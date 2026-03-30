# Computational Methods for Data Driven Earth Sciences

This repository hosts course materials for a course on computational methods for earth sciences. The course is taught as HWRS640 at the University of Arizona by Dr. Andrew Bennett, but materials are free for all to use to learn, adapt, and remix.

## 📚 Course Website

Visit the course website: [https://hwrs640.github.io/computational_methods_course/](https://hwrs640.github.io/computational_methods_course/)

You may also find the [minicamels package](https://github.com/BennettHydroLab/minicamels) useful for working with the CAMELS datasets in Python.

## 📖 Course Structure

The course materials are organized into the following sections:

- **Notebooks**: Interactive Jupyter notebooks with code examples and exercises
- **Slides**: Lecture slides and presentations
- **Resources**: Additional reading materials and reference guides
- **Data**: Datasets used throughout the course
- **Extras**: Supplementary materials and advanced topics

## 🚀 Getting Started

### Viewing the Course Materials

The easiest way to access the course materials is through the published JupyterBook website linked above.

### Running the Notebooks

If you want to run the notebooks and work with the code interactively, there are two options:

#### Option 1: GitHub Codespaces (Recommended)

The easiest way to get a fully configured environment is through GitHub Codespaces. This gives you a cloud-based VS Code editor with all dependencies pre-installed.

1. Navigate to the [repository on GitHub](https://github.com/hwrs640/computational_methods_course)
2. Click the green **Code** button, then select the **Codespaces** tab
3. Click **Create codespace on main**

The environment will automatically set up a Python virtual environment, install all dependencies, and register the `Python (HWRS640)` Jupyter kernel. Once the setup completes, open any notebook in the `notebooks/` directory and select the `Python (HWRS640)` kernel to get started.

#### Option 2: Local Setup

To run the notebooks on your own machine you will need Python 3.13+ and [uv](https://docs.astral.sh/uv/) installed.

1. Clone this repository:
   ```bash
   git clone https://github.com/hwrs640/computational_methods_course.git
   cd computational_methods_course
   ```

2. Create the virtual environment and install dependencies:
   ```bash
   uv venv
   uv sync
   ```

3. Activate the environment and register the Jupyter kernel:
   ```bash
   source .venv/bin/activate
   python -m ipykernel install --user --name hwrs640 --display-name "Python (HWRS640)"
   ```

4. Launch JupyterLab:
   ```bash
   jupyter lab
   ```

Open any notebook in the `notebooks/` directory and select the `Python (HWRS640)` kernel.

### Building the JupyterBook

To build the course website locally:

1. Clone this repository:
   ```bash
   git clone https://github.com/hwrs640/computational_methods_course.git
   cd computational_methods_course
   ```

2. Install the required dependencies:
   ```bash
   pip install -r requirements.txt
   ```

3. Build the book:
   ```bash
   jupyter-book build .
   ```

4. View the built book:
   ```bash
   open _build/html/index.html
   ```
   (On Linux, use `xdg-open` instead of `open`)

## 📝 Contributing

Contributions to improve the course materials are welcome! Please follow these steps:

1. Fork the repository
2. Create a new branch for your changes
3. Make your changes and test the build
4. Submit a pull request

## 🔄 Deployment

The JupyterBook is automatically built and deployed to GitHub Pages when changes are pushed to the `main` branch via GitHub Actions.

## 📄 License

This course material is provided for educational purposes.
