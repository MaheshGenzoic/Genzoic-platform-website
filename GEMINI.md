# Project Overview

This is a React-based web application for the Genzoic platform, a company that provides AI agents and automation solutions for businesses. The website showcases their services, features a marketplace of AI agents, and includes a blog.

- **Framework:** React 18 with TypeScript
- **Build Tool:** Vite
- **Styling:** Tailwind CSS with shadcn/ui components
- **Routing:** React Router DOM
- **Deployment:** Vercel

The project is well-structured, with a clear separation of concerns. The `src` directory contains the main application logic, with subdirectories for components, pages, hooks, and utilities.

# Building and Running

To get the project up and running, follow these steps:

1.  **Install dependencies:**
    ```bash
    npm install
    ```
    or if you are using bun:
    ```bash
    bun install
    ```

2.  **Run the development server:**
    ```bash
    npm run dev
    ```
    or if you are using bun:
    ```bash
    bun dev
    ```
    The application will be available at `http://localhost:8080`.

3.  **Build for production:**
    ```bash
    npm run build
    ```

4.  **Lint the code:**
    ```bash
    npm run lint
    ```

# Development Conventions

- **Styling:** Use Tailwind CSS classes for styling. Adhere to the existing styling patterns and use `shadcn/ui` components whenever possible.
- **Components:** Reusable components are located in `src/components`. Page-specific components are in `src/pages`.
- **Data:** Blog content and agent data are stored statically in TypeScript files within the `src/content` and `src/utils` directories, respectively.
- **Routing:** The main routing is defined in `src/App.tsx`. To add a new page, create a new component in `src/pages`, and add a new route in `src/App.tsx`.
- **Images:** Local images are used for logos and UI assets, while content images for blogs and agents are loaded from external Unsplash URLs.
