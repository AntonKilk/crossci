<script lang="ts">
  import { navigate } from "svelte-routing";

  function loginAsTeacher() {
    navigate("/teacher");
  }

  function loginAsStudent() {
    navigate("/student");
  }
</script>

<div class="login-container">
  <img src="/logo.webp" alt="Logo" />
  <h2>
    #CrosSci is an innovative platform that uses GenAI to create
    cross-disciplinary problem-solving experiences, helping students connect
    concepts across STEAM fields
  </h2>

  <div class="buttons">
    <button on:click={loginAsTeacher}>Login as Teacher</button>
    <button on:click={loginAsStudent}>Login as Student</button>
  </div>
  <div class="inline-paragraphs">
    <h2>Dynamic Real-Word Challenge Generation</h2>
    <h2>Personalized Learning Paths</h2>
    <h2>Comprehensive Feedback</h2>
  </div>
</div>

<style>
  img {
    max-width: 600px;
    height: auto;
  }
  .inline-paragraphs {
    padding-top: 40px;
    display: flex;
    gap: 20px;
  }

  .login-container {
    max-width: 600px;
    margin: 100px auto;
    text-align: center;
  }

  h2 {
    font-size: 1.2em;
    color: #7f8c8d;
    margin-bottom: 2em;
  }

  .buttons {
    display: flex;
    gap: 20px;
    justify-content: center;
  }

  button {
    padding: 15px 30px;
    font-size: 1.1em;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    transition: transform 0.2s;
  }

  button:first-child {
    background-color: #3498db;
    color: white;
  }

  button:last-child {
    background-color: #229954;
    color: white;
  }

  button:hover {
    transform: scale(1.05);
  }
</style>
