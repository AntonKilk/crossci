<script lang="ts">
  import { navigate } from "svelte-routing";
  import { marked } from "marked";
  import { GoogleGenerativeAI } from "@google/generative-ai";
  import prompt from "../../prompt.txt?raw";

  let message = "";
  let chatHistory: Array<{ text: string; sender: "user" | "ai" }> = [];
  const apiKey = import.meta.env.VITE_GOOGLE_API_KEY;

  let currentTask = {
    name: "Problem 1: Sustainable Building Design and Material Selection",
    scenario:
      "You are working as a junior civil engineer for a company tasked with designing a small community center in a rural Estonian village. The community has requested that the building be as environmentally friendly as possible while remaining cost-effective.",
    content:
      "Develop a proposal for the community center that focuses on sustainable construction practices.",
    steps: [
      "Research and select eco-friendly building materials (e.g., recycled concrete, timber, insulation)",
      "Explain the advantages and limitations of your chosen materials in terms of durability, cost, and environmental impact",
      "Use basic calculations to estimate the cost savings and environmental benefits (e.g., reduction in CO₂ emissions)",
    ],
  };

  async function sendMessage() {
    if (message.trim()) {
      chatHistory = [...chatHistory, { text: message, sender: "user" }];
      try {
        const genAI = new GoogleGenerativeAI(apiKey);
        const model = genAI.getGenerativeModel({ model: "gemini-1.5-flash" });
        const result = await model.generateContent([prompt, message]);
        if (result.response) {
          const data = await result.response.text();
          const formattedData = await marked(data);
          chatHistory = [...chatHistory, { text: formattedData, sender: "ai" }];
        } else {
          chatHistory = [
            ...chatHistory,
            { text: "Failed to get response from Gemini API.", sender: "ai" },
          ];
        }
      } catch (error) {
        chatHistory = [
          ...chatHistory,
          { text: "Cannot access Gemini API.", sender: "ai" },
        ];
      }

      message = "";
    }
  }

  function quit() {
    navigate("/");
  }
</script>

<div class="student-workspace">
  <header>
    <div class="header-content">
      <div>
        <h1>Your Current Task</h1>
        <h2>{currentTask.name}</h2>
      </div>
      <button class="quit-button" on:click={quit}>Quit</button>
    </div>
  </header>

  <div class="task-content">
    <h3 class="left-align">Scenario</h3>
    <p>{currentTask.scenario}</p>
    <h3 class="left-align">Task</h3>
    <p>{currentTask.content}</p>
    <h3 class="left-align">Key Steps</h3>
    <ul class="left-align">
      {#each currentTask.steps as step}
        <li>{step}</li>
      {/each}
    </ul>
  </div>

  <div class="chat-section">
    <div class="chat-history">
      {#each chatHistory as message}
        <div class="message {message.sender}">
          {@html message.text}
        </div>
      {/each}
    </div>

    <div class="chat-input">
      <input
        type="text"
        bind:value={message}
        placeholder="Ask a question about your task..."
        on:keypress={(e) => e.key === "Enter" && sendMessage()}
      />
      <button on:click={sendMessage}>Send</button>
    </div>
  </div>
</div>

<style>
  .left-align {
    text-align: left;
  }
  .student-workspace {
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
    background-color: var(--background-color, #ffffff);
    color: var(--text-color, #000000);
  }

  header {
    margin-bottom: 30px;
  }

  .header-content {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .quit-button {
    background-color: #e74c3c;
    color: var(--button-text-color, white);
    padding: 10px 20px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.2s;
  }

  .quit-button:hover {
    background-color: #c0392b;
  }

  .task-content {
    background: var(--card-background-color, #ffffff);
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    margin-bottom: 30px;
    color: var(--text-color, #000000);
  }

  .chat-section {
    background: var(--card-background-color, #ffffff);
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    padding: 20px;
    color: var(--text-color, #000000);
  }

  .chat-history {
    height: 300px;
    overflow-y: auto;
    margin-bottom: 20px;
    padding: 10px;
  }

  .message {
    padding: 10px;
    margin: 5px;
    border-radius: 8px;
    max-width: 80%;
  }

  .message.user {
    background: #3498db;
    color: white;
    margin-left: auto;
  }

  .message.ai {
    background: #f0f0f0;
    color: #333;
  }

  .chat-input {
    display: flex;
    gap: 10px;
  }

  input {
    flex: 1;
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 4px;
  }

  button {
    padding: 10px 20px;
    background: #3498db;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }

  button:hover {
    background: #2980b9;
  }

  h1,
  h2 {
    color: var(--heading-color, #2c3e50);
    margin: 0;
  }
</style>
