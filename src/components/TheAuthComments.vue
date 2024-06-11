<template>
  <section class="card">
    <div class="container">
      <h2><i>Wedding Chat and Greetings</i></h2>
       <!-- Comment and Greetings Form-->
       <div class="py-4">
        <form @submit.prevent="addComment" class="max-w-xl mx-auto">
          <label for="doa" class="block mb-2 text-sm font-medium text-gray-900 ">Comments and Questions</label>
          <div class="relative">
            <textarea id="doa" v-model="newCommentText" placeholder="Write your comments here" rows="4" class="block p-2.5 w-full text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500 dark:border-gray-600 dark:placeholder-gray-400 dark:focus:ring-blue-500 dark:focus:border-blue-500"></textarea>
            <div class="absolute bottom-0 right-0 py-1 px-1">
              <button type="submit" class="py-1 px-2 rounded-l-lg outline outline-gray-800 outline-2 outline-offset-2 bg-green-600 text-gray-100">Send a Greeting</button>
            </div>
          </div>
        </form>
        <div class="py-2 px-1">
          <button class="py-1 px-2 rounded-lg outline outline-gray-800 outline-2 outline-offset-2 bg-green-600 text-gray-100" v-if="user" @click="logout">Logout</button>
          <button class="py-1 px-2 rounded-lg outline outline-gray-800 outline-2 outline-offset-2 bg-green-600 text-gray-100" v-else @click="handleGoogleLogin">Login with Google</button>
        </div>
      </div>
        <ul v-if="comments.length">
          <li v-for="comment in comments" :key="comment.id">
            <div class="comment py-1 my-1">
                <img :src="comment.photoURL" alt="Avatar" class="comment-avatar">
              <div class="comment-info">
                <div class="comment-name">{{ comment.displayName }}</div>
                <div class="comment-text">{{ comment.text }}</div>
                <div class="comment-timestamp">{{ formatTimestamp(comment.time) }}</div>
                <div class="button_comment flex gap-4 py-1 my-1">
                  <button class="text-sm" @click="toggleReplyForm(comment)">Reply</button>
                  <button class="text-sm" @click="deleteComment(comment.id)">Delete</button>
                  <button class="text-sm" @click="toggleReplies(comment)">{{ comment.showReplies ? 'Hide replies' : 'Show replies [' + comment.replies.length + ']' }}</button>
                </div>
              </div>
            </div>

            <!-- Shoe reply form for comments -->
            <div class="pb-4" v-if="comment.replying">
              <div class="comment.replying relative">
                  <textarea v-model="comment.replyText" placeholder="Reply to this comment" class="text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500 dark:border-gray-600 dark:placeholder-gray-400 dark:focus:ring-blue-500 dark:focus:border-blue-500"></textarea>
                <div class="absolute bottom-0 right-0 py-1 px-1">
                  <button class="py-0.5 px-1 text-sm rounded-l-lg outline outline-gray-800 outline-2 outline-offset-2 bg-green-500 text-gray-100" @click="sendReply(comment)">Send Reply</button>
                </div>
              </div>
            </div>

            <!-- Show reply under comments -->
            <div class="reply_comment py-1">
              <ul v-show="comment.showReplies && comment.replies && comment.replies.length">
                <li v-for="reply in comment.replies" :key="reply.id">
                  <div class="comment">
                    <img :src="reply.photoURL" alt="Avatar" class="comment-avatar_reply">
                    <div class="comment-info">
                      <div class="comment-name">{{ reply.displayName }}</div>
                      <div class="comment-text">{{ reply.text }}</div>
                      <div class="comment-timestamp">{{ formatTimestamp(reply.time) }}</div>
                      <button @click="deleteReply(comment.id, reply.id)">Delete</button>
                    </div>
                  </div>
                </li>
              </ul>
            </div>
          </li>
        </ul>
    </div>
  </section>
</template>

<script>
import { initializeApp } from 'firebase/app';
import { getAuth, signInWithPopup, GoogleAuthProvider } from "firebase/auth";
import { ref } from 'vue';
import axios from 'axios';

// Configure Firebase
const firebaseConfig = {
  apiKey: import.meta.env.VITE_FIREBASE_API_KEY,
  authDomain: import.meta.env.VITE_FIREBASE_AUTH_DOMAIN,
  projectId: import.meta.env.VITE_FIREBASE_PROJECT_ID,
  storageBucket: import.meta.env.VITE_FIREBASE_STORAGE_BUCKET,
  messagingSenderId: import.meta.env.VITE_FIREBASE_MESSAGING_SENDER_ID,
  appId: import.meta.env.VITE_FIREBASE_APP_ID,
};

// Initialize Firebase App
const firebaseApp = initializeApp(firebaseConfig);
const auth = getAuth(firebaseApp);
const provider = new GoogleAuthProvider();

export default {
  data() {
    return {
      comments: [],
      user: null,
      newCommentText: '',
    };
  },
  methods: {
    async handleGoogleLogin() {
      try {
        const result = await signInWithPopup(auth, provider);
        this.user = result.user;
        console.log("Successfully logged in with Google:");
      } catch (error) {
        console.error("Error when logging in with Google:", error.message);
      }
    },
    async addComment() {
      if (!this.newCommentText.trim()) return;
      try {
        if (!this.user) {
          // Directly navigate to the login pop-up
        await this.handleGoogleLogin();
        return;
        }
        const commentData = {
          text: this.newCommentText,
          displayName: this.user.displayName,
          photoURL: this.user.photoURL,
          timestamp: new Date()
        };
        const idToken = await this.user.getIdToken(); // Retrieves JWT token from Firebase Authentication
        const urlPost = import.meta.env.VITE_API_URL
        const response = await axios.post(`${urlPost}`, commentData, {
          headers: {
            Authorization: `Bearer ${idToken}` // Added JWT token to Authorization header
          }
        });
        console.log('Comment successfully added:', response.data);
        this.newCommentText = ''; // Clear comment text
        // Retrieve comments after adding a new comment
        await this.getComments();
        alert('Thank you, your comment has been submitted successfully!');
      } catch (error) {
        // console.error('Error adding comment: ', error.response.data);
        alert('Error occurred: ' + JSON.stringify(error.response.data.error));
      }
    },
    toggleReplyForm(comment) {
      comment.replying = !comment.replying; // Toggle status replying
      comment.replyText = ''; // Reset reply text
    },
// Reply Comment
async sendReply(comment) {
  try {
    // Check if the user is logged in
    if (!this.user) {
      // If not, navigate to the login process
      await this.handleGoogleLogin();
      return;
    }

    // Comment text validation cannot be empty or contain only spaces
    if (!comment.replyText || !comment.replyText.trim()) {
      alert('Invalid comment text');
      return;
    }

    // Validate special characters in comment text
    const forbiddenCharsRegex = /[@#$%^&*()":{}|<>]/;
    if (forbiddenCharsRegex.test(comment.replyText)) {
      alert('Comment text contains special characters that are not allowed');
      return;
    }

    // Send reply data to the backend
    const urlPost = import.meta.env.VITE_API_URL
    const idToken = await this.user.getIdToken(); // Retrieve JWT token from Firebase Authentication
    const response = await axios.post(`${urlPost}`, {
      text: comment.replyText,
      parentId: comment.id, // Include the parentId of the comment to reply to
      displayName: this.user.displayName,
      photoURL: this.user.photoURL
    }, {
      headers: {
        Authorization: `Bearer ${idToken}` // Add JWT token to Authorization header
      }
    });

    // Update the frontend view
    comment.replying = false;
    comment.replyText = ''; // Clear reply text after sending
    await this.getComments();
    // If successful, display a success message
    console.log('Reply successfully added:', response.data.message);
    alert('Reply successfully added:', response.data);
  } catch (error) {
    console.error('Error sending reply:', error);
    alert('Failed to send reply. Please try again.' + error.message);
  }
},

async deleteComment(commentId) {
    try {
      // User confirmation before deleting comments
      const confirmation = confirm('Are you sure you want to delete this comment?');
      if (!confirmation) {
        return; // If user cancels deletion
      }

      // Retrieve JWT token from Firebase Authentication
      const idToken = await this.user.getIdToken();

      // Send a DELETE request to the backend
      const urlDelete = import.meta.env.VITE_API_URL
      const response = await axios.delete(`${urlDelete}/${commentId}`, {
        headers: {
          Authorization: `Bearer ${idToken}` // Add JWT token to Authorization header
        }
      });

      // Update the view after a comment has been successfully deleted
      console.log('Comment successfully deleted:', response.data.message);
      alert('Comment successfully deleted' + response.data);

      // Refresh comment data after successful deletion
      if (response.data && response.data.error) {
          console.error(response.data.error);
          // Display an error message on the UI
        } else {
          await this.getComments();
        }
    } catch (error) {
      console.error('Error deleting comment:', error.response.data.error);
      alert('Error deleting comment: ' + error.response.data.error);
    }
  },
  // Delete reply comment
  async deleteReply(commentId, replyId) {
    try {
      // User confirmation before deleting replies
      const confirmation = confirm('Are you sure you want to delete this reply?');
      if (!confirmation) {
        return; // If the user cancel the deletion
      }

      // Retrieve JWT token from Firebase Authentication
      const idToken = await this.user.getIdToken();

      // Send a DELETE request to the backend
      const urlDelete = import.meta.env.VITE_API_URL
      const response = await axios.delete(`${urlDelete}/${commentId}/replies/${replyId}`, {
        headers: {
          Authorization: `Bearer ${idToken}` // Add JWT token to Authorization header
        }
      });

      // Update view after reply has been successfully deleted
      console.log('Reply successfully deleted:', response.status);
      alert('Reply successfully deleted' + response.status);
      if (response.data && response.data.error) {
          console.error(response.data.error);
          // Display an error message on the UI
        } else {
          await this.getComments();
        }
      // Refresh comment data after successful deletion
      // await this.getComments();
    } catch (error) {
      if (error.response && error.response.data && error.response.data.payload && error.response.data.payload.message) {
        alert('Error occurred: ' + error.response.data.payload.message);
      } else {
        console.error('Error deleting reply:', error);
        alert('An error occurred while deleting a reply');
      }
    }
  },
    async logout() {
    try {
      await auth.signOut();
      this.user = null;
      console.log("Logout successful.");

      alert("Logout");
    } catch (error) {
      console.error('Error logging out: ', error);
      alert('Error logging out: ' + error.message);
    }
  },
  formatTimestamp(timestamp) {
  const now = new Date();
  const date = new Date(timestamp._seconds * 1000);

  // Check if the timestamp is today
  const isToday = date.getDate() === now.getDate() &&
                 date.getMonth() === now.getMonth() &&
                 date.getFullYear() === now.getFullYear();

  // Check if the timestamp is yesterday
  const yesterday = new Date(now);
  yesterday.setDate(now.getDate() - 1);
  const isYesterday = date.getDate() === yesterday.getDate() &&
                      date.getMonth() === yesterday.getMonth() &&
                      date.getFullYear() === yesterday.getFullYear();

  if (isToday) {
    let hours = date.getHours();
    const minutes = ('0' + date.getMinutes()).slice(-2);
    const seconds = ('0' + date.getSeconds()).slice(-2);
    const ampm = hours >= 12 ? 'PM' : 'AM';
    hours = hours % 12;
    hours = hours ? hours : 12; // Handle midnight

    return `Today at ${hours}:${minutes} ${ampm}`;
  } else if (isYesterday) {
    let hours = date.getHours();
    const minutes = ('0' + date.getMinutes()).slice(-2);
    const seconds = ('0' + date.getSeconds()).slice(-2);
    const ampm = hours >= 12 ? 'PM' : 'AM';
    hours = hours % 12;
    hours = hours ? hours : 12; // Handle midnight

    return `Yesterday at ${hours}:${minutes} ${ampm}`;
  } else {
    // Regular date format
    const year = date.getFullYear();
    const month = ('0' + (date.getMonth() + 1)).slice(-2);
    const day = ('0' + date.getDate()).slice(-2);
    let hours = date.getHours();
    const minutes = ('0' + date.getMinutes()).slice(-2);
    const seconds = ('0' + date.getSeconds()).slice(-2);
    const ampm = hours >= 12 ? 'PM' : 'AM';
    hours = hours % 12;
    hours = hours ? hours : 12; // Handle midnight

    const formattedTime = `${day}/${month}/${year} at ${hours}:${minutes} ${ampm}`;

    return formattedTime;
  }
},
    async getComments() {
      try {
        const urlGet = import.meta.env.VITE_API_URL
        const response = await axios.get(`${urlGet}`);
        if (response.status === 200) {
          this.comments = response.data.payload.data;
        } else {
          console.error('Failed to get comments:', response.data.message);
        }
      } catch (error) {
        console.error('Error getting comments:', error);
      }
    },
  },
  async mounted() {
    try {
      // Retrieve comments when components are loaded
      await this.getComments();
    } catch (error) {
      console.error('Error fetching comments:', error);
    }
    // Add a listener for Firebase login status
    auth.onAuthStateChanged(user => {
      this.user = user;
    });
  }
};
</script>
<style scoped>
@import url('https://use.typekit.net/fyp0lzh.css');

@font-face {
  font-family: 'Mier A';
  src: url('src/assets/mier/MierA-Regular.eot');
  src: local('Mier A Regular'), local('MierA-Regular'),
  url('src/assets/mier/MierA-Regular.eot?#iefix') format('embedded-opentype'),
  url('src/assets/mier/MierA-Regular.woff2') format('woff2'),
  url('src/assets/mier/MierA-Regular.woff') format('woff'),
  url('src/assets/mier/MierA-Regular.ttf') format('truetype');
  font-weight: normal;
  font-style: normal;
}

@font-face {
  font-family: 'Mier A Demi';
  src: url('src/assets/mier/MierA-DemiBold.eot');
  src: local('Mier A DemiBold'), local('MierA-DemiBold'),
  url('src/assets/mier/MierA-DemiBold.eot?#iefix') format('embedded-opentype'),
  url('src/assets/mier/MierA-DemiBold.woff2') format('woff2'),
  url('src/assets/mier/MierA-DemiBold.woff') format('woff'),
  url('src/assets/mier/MierA-DemiBold.ttf') format('truetype');
  font-weight: 600;
  font-style: normal;
}

h2 {
  text-align: center;
  font-family: "freight-big-pro", serif !important;
  font-size: 39px;
}

label {
  text-align: center;
  font-family: "Mier A Demi", sans-serif !important;
  font-style: normal;
  font-size: large;
}

p {
  text-align: center;
  font-family: "Mier A", sans-serif !important;
  font-size: medium;
}

.comment {
  display: flex;
}

.comment\.replying {
    display: flex;
    flex-direction: column;
    flex-direction: column;
    padding-left: 3rem;
}

.reply_comment {
    padding-left: 3rem;
}

.comment-container {
  display: flex;
  align-items: flex-start; /* Change the vertical position up */
  margin-bottom: 10px;
}

.comment-avatar {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  margin-right: 10px;
}

.comment-avatar_reply{
  width: 30px;
  height: 30px;
  border-radius: 50%;
  margin-right: 10px;
}

.comment-info {
  flex: 1;
}

.comment-name {
  font-weight: bold;
}

.comment-text {
  margin-bottom: 5px;
}

.comment-timestamp {
  font-size: 0.8rem;
  color: #888;
}

form{
  display: flex;
  flex-direction: column;
}

.buttonSubmit{
  text-align: start;
}

section{
  text-align: start;
}
</style>