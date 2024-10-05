# Vue Todo List

## Overview

This project is a Todo List application built with Vue.js, Tailwind CSS, and Supabase. It enables users to manage tasks with functionalities for creating, updating, and deleting tasks.

## Technologies Used

- **Vue.js**: A progressive JavaScript framework for building user interfaces.
- **Tailwind CSS**: A utility-first CSS framework for building responsive and modern designs.
- **Supabase**: An open-source alternative to Firebase, Supabase provides a database backend for storing tasks.

The `AddTask` component handles the creation and display of tasks. It includes a form for adding new tasks and a list to display existing tasks. The component leverages Supabase for task storage.

### Features:

- Form for creating or updating tasks.
- Display of the task list.
- Tasks fetched from Supabase on component mount.

### Installation and Running Locally

1. **Clone the repository:**

   ```bash
   git clone https://github.com/your-username/vue-todo-list.git
   cd vue-todo-list
   ```

2. **Install Dependencies:**

   ```bash
   npm install
   ```

3. **Run Locally:**

   ```bash
   npm run dev
   ```

   Open your browser and navigate to [http://localhost:3000](http://localhost:3000).
