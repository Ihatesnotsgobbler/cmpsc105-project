# CMPSC 105 Data Exploration - Project Setup

## Overview
In this lab, we will setup your environment for the project. 

You will be working with your project team for this lab.

## Part 1: The GitHub Workflow
In this course, our cadence for managing assignments relies on GitHub. Before we write any code, we need to set up your environment. Follow these steps:

1. **Fork the Repository:** Navigate to the provided lab repository link. In the top right corner, click the **Fork** button. This creates a personal copy of the repository under your own GitHub account.
2. **Clone the Repository:** Open your terminal in VS Code and clone your *forked* version of the repository to your local machine:
   ```bash
   git clone https://github.com/YOUR_USERNAME/cmpsc105-project.git
   ```
3. **Navigate and Open:** Open the `cmpsc105-project` folder in VS Code.

## Part 2: Setting up the Python Environment with `uv`
Before we run our analysis, we need an isolated environment to manage our dependencies. We use `uv`, an extremely fast Python package manager.

1. **Create the Virtual Environment:** In your terminal, ensure you are inside the `cmpsc105-project` directory, then run:
   ```bash
   uv venv
   ```
2. **Activate the Environment:** You must activate the environment so your terminal uses the isolated Python version.
   * On **macOS/Linux**:
     ```bash
     source .venv/bin/activate
     ```
   * On **Windows**:
     ```bash
     .venv\Scripts\activate
     ```
   If you run into an issue, try typing 
   ```bash
   uv init
   ```

3. **Install Libraries:** With the environment active, install the required library:
   ```bash
   uv add pandas
   uv add seaborn
   ```

## Part 3: Download your data
Download the data from the provided link and place it in the `cmpsc105-project` directory. Note that you should not commit the data to your repository. Instead, add the data file to your `.gitignore` file to prevent it from being tracked by Git.

Go to the `.gitignore` file and add the following line:
```
data.csv
``` 
Remember to replace `data.csv` with the actual name of your data file.`


## Part 4: Getting to Work
Create an `analysis.ipynb` notebook file to begin your analysis.

## Note: Keeping track of your work
As you work on your analysis, it's important to keep track of your changes. Use GitHub to manage your work. In VS Code, you can use the Source Control panel to stage, commit, and push your changes to your forked repository.
