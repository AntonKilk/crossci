<script lang="ts">
  import { navigate } from "svelte-routing";
  import { Line } from 'svelte-chartjs';
  import {
    Chart as ChartJS,
    CategoryScale,
    LinearScale,
    PointElement,
    LineElement,
    Title,
    Tooltip,
    Legend
  } from 'chart.js';

  ChartJS.register(
    CategoryScale,
    LinearScale,
    PointElement,
    LineElement,
    Title,
    Tooltip,
    Legend
  );

  const studentData = {
    labels: ['Week 1', 'Week 2', 'Week 3', 'Week 4', 'Week 5'],
    datasets: [
      {
        label: 'Student A',
        data: [65, 75, 82, 85, 90],
        borderColor: 'rgb(75, 192, 192)',
      },
      {
        label: 'Student B',
        data: [60, 65, 68, 70, 72],
        borderColor: 'rgb(255, 99, 132)',
      },
      {
        label: 'Student C',
        data: [70, 78, 85, 88, 92],
        borderColor: 'rgb(54, 162, 235)',
      }
    ]
  };

  const options = {
    responsive: true,
    plugins: {
      legend: {
        position: 'top' as const,
      },
      title: {
        display: true,
        text: 'Student Progress Over Time'
      }
    }
  };

  const recommendedTasks = [
    {
      level: 'Advanced',
      students: ['Student A', 'Student C'],
      tasks: [
        {
          header: 'Problem 1: Implement a Neural Network from Scratch',
          scenario: 'You are part of a research team tasked with developing a new autonomous rover for planetary exploration. The rover must analyze images from its camera to identify terrain types, such as rocky, sandy, or smooth surfaces. Due to constraints, you cannot use existing machine learning frameworks.',
          content: 'Implement a simple neural network from scratch using Python (or any preferred language) to classify terrain types based on grayscale image data.',
          requirement: [
            'Design the architecture (input, hidden, and output layers).',
            'Implement forward and backward propagation.',
            'Use gradient descent for optimization.',
            'Evaluate performance using synthetic datasets provided.'
          ]        
        },
        {
          header: 'Problem 2: Design an Optimization Algorithm',
          scenario: 'You are consulting for a logistics company that wants to optimize delivery routes in a city with heavy traffic. Existing algorithms do not perform well under fluctuating traffic conditions. The company needs a custom optimization algorithm tailored to their data.',
          content: 'Design an optimization algorithm to minimize delivery time while considering constraints like traffic congestion, package priorities, and vehicle capacity.',
          requirement: [
            'Use a gradient-based or heuristic approach (e.g., genetic algorithms, simulated annealing).',
            'Implement a simulation to test the algorithm\'s efficiency.',
            'Analyze performance with multiple traffic datasets.'
          ]        
        }
      ]
    },
    {
      level: 'Intermediate',
      students: ['Student B'],
      tasks: [
        {
          header: 'Problem 1: Basic Machine Learning Concepts',
          scenario: 'A local art gallery wants to predict the popularity of different art pieces based on features like color palette, size, and historical significance. They have collected a dataset but need a basic model to get started.',
          content: 'Build a simple machine learning model to predict the popularity score of art pieces.',
          requirement: [
            'Understand and explain the dataset features.',
            'Implement a linear regression model using a library like scikit-learn.',
            'Evaluate the model using metrics like mean squared error.'
          ]        
        },
        {
          header: 'Problem 2: Data Preprocessing Techniques',
          scenario: 'An environmental organization is using sensors to monitor air quality in different regions. However, the data contains missing values, outliers, and inconsistent formats.',
          content: 'Preprocess the dataset to prepare it for further analysis.',
          requirement: [
            'Handle missing data (imputation or removal).',
            'Detect and address outliers.',
            'Normalize and scale the features.',
            'Visualize the cleaned data to confirm improvements.'
          ]        
        }
      ]
    }
  ];

  function quit() {
    navigate("/");
  }
</script>

<div class="teacher-dashboard">
  <div class="header">
    <h2>Your course statistics</h2>
    <button class="quit-button" on:click={quit}>Quit</button>
  </div>
  
  <div class="chart-container">
    <Line data={studentData} options={options} />
  </div>

  <div class="recommendations">
    <h2>Recommended Tasks</h2>
    {#each recommendedTasks as group}
      <div class="task-group">
        <h3>{group.level} Group</h3>
        <p>Students: {group.students.join(', ')}</p>
        {#each group.tasks as task}
        <div class="task">
          <h4>{task.header}</h4>
          <p><strong>Scenario:</strong> {task.scenario}</p>
          <p><strong>Task:</strong> {task.content}</p>
          <p class="key-requirements"><strong>Key Requirements:</strong></p>
            <ul class="key-requirements">
              {#each task.requirement as req}
                <li>{req}</li>
              {/each}
            </ul>
        </div>
      {/each}
      </div>
    {/each}
  </div>
</div>

<style>
  .teacher-dashboard {
    padding: 20px;
    min-width: 480px;
    max-width: 800px;
    margin: 0 auto;
  }

  .header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
  }

  .quit-button {
    background-color: #e74c3c;
    color: white;
    padding: 10px 20px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.2s;
  }

  .quit-button:hover {
    background-color: #c0392b;
  }

  .chart-container {
    margin: 20px 0;
    padding: 20px;
    background: white;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  }

  .recommendations {
    margin-top: 40px;
  }

  .task-group {
    text-align: left;
    background: white;
    padding: 20px;
    margin: 20px 0;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  }

  h1, h2 {
    color: #2c3e50;
    margin: 0;
  }

  h3 {
    color: #3498db;
  }

  ul {
    list-style-type: none;
    padding: 0;
  }

  li {
    padding: 10px 0;
    border-bottom: 1px solid #eee;
  }
</style>