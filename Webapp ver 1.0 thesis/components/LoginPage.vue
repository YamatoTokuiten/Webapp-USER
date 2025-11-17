<template>
  <div class="container" v-show="visible">
    <div class="guide">
      <p><strong>Username</strong> = Plate Number</p>
      <p><strong>Password</strong> = Receipt Code</p>
    </div>

    <h2>Login</h2>

    <div class="input-group">
      <input v-model="username" type="text" placeholder="Enter plate number" autocomplete="username" />
    </div>

    <div class="input-group">
      <input :type="showPassword ? 'text' : 'password'" v-model="password" placeholder="Enter password" autocomplete="current-password" />
      <span id="togglePassword" @click="togglePassword" :style="{color: showPassword ? '#27ae60' : 'gray' }">&#128065;</span>
    </div>

    <button @click="confirm">Confirm</button>
    <div class="message" :style="{color: messageColor}">{{ message }}</div>
  </div>
</template>

<script>
export default {
  name: 'LoginPage',
  data() {
    return {
      username: '',
      password: '',
      showPassword: false,
      message: '',
      messageColor: '#e74c3c',
      visible: false,
    };
  },
  mounted() {
    // Fade in effect
    this.visible = true;
  },
  methods: {
    togglePassword() {
      this.showPassword = !this.showPassword;
    },
    confirm() {
      const validUsername = 'KMY 420';
      const validPassword = 'Mikatrix';

      const usernameValid = this.username.trim().toUpperCase() === validUsername;
      const passwordValid = this.password === validPassword;

      if (!usernameValid && !passwordValid) {
        this.message = 'Error, invalid credentials';
        this.messageColor = '#e74c3c';
      } else if (!usernameValid) {
        this.message = 'Error, invalid username';
        this.messageColor = '#e74c3c';
      } else if (!passwordValid) {
        this.message = 'Error, invalid password';
        this.messageColor = '#e74c3c';
      } else {
        this.message = 'Login successful!';
        this.messageColor = '#27ae60';
        sessionStorage.setItem('plateNumber', this.username.trim());
        // Simulate navigation delay
        setTimeout(() => {
          this.$emit('login-success');
        }, 1000);
      }
    },
  },
};
</script>

<style scoped>
.container {
  width: 400px;
  max-width: 95vw;
  background: white;
  border-radius: 12px;
  box-shadow: 0 8px 24px rgba(0,0,0,0.1);
  padding: 30px 40px;
  text-align: center;
  opacity: 0;
  transition: opacity 0.8s ease;
}
.container[v-show="true"] {
  opacity: 1;
}

.guide {
  background-color: #fcf9f3;
  padding: 15px;
  border-radius: 6px;
  margin-bottom: 25px;
  color: #555;
  font-size: 1.14em;
  line-height: 1.5;
  text-align: center;
}
.guide strong {
  color: #2c3e50;
}

h2 {
  color: #2c3e50;
  margin-bottom: 20px;
  font-size: 1.9em;
  font-weight: 700;
}

.input-group {
  position: relative;
  margin-bottom: 25px;
}

input[type=text],
input[type=password],
input[type=password="text"] {
  width: 100%;
  padding: 14px 45px 14px 14px;
  font-size: 1.2em;
  border: 2px solid #ddd;
  border-radius: 6px;
  transition: border-color 0.3s;
  box-sizing: border-box;
}

input[type=text]:focus,
input[type=password]:focus {
  border-color: #27ae60;
  outline: none;
}

#togglePassword {
  position: absolute;
  top: 50%;
  right: 15px;
  transform: translateY(-50%);
  font-size: 1.3em;
  cursor: pointer;
  user-select: none;
}

button {
  width: 100%;
  padding: 14px 0;
  background-color: #27ae60;
  border: none;
  border-radius: 8px;
  font-weight: 600;
  font-size: 1.25em;
  color: white;
  cursor: pointer;
  transition: background-color 0.25s;
}
button:hover {
  background-color: #219150;
}

.message {
  margin-top: 15px;
  font-weight: bold;
  font-size: 1.1em;
  min-height: 28px;
}
</style>
