# CS 499 Computer Science Capstone Portfolio

Welcome to my Capstone ePortfolio repository for CS 499. Over the course of my computer science degree, I built a solid technical foundation, but for this capstone, I wanted to take things a step further. I selected three distinct projects from earlier in my coursework and went back through them to thoroughly refactor the code, fix hidden vulnerabilities, optimize performance, and bring everything up to professional standards.

This repository serves as a comprehensive showcase of that work, detailing the artifacts I chose, the specific issues I identified in the original code, and the enhancements implemented to make them production-ready.

---

## Table of Contents
- [Artifact 1: Software Engineering and Design](#artifact-1-software-engineering-and-design-backend-services--testing)
- [Artifact 2: Computational Graphics and Performance Optimization](#artifact-2-computational-graphics-and-performance-optimization-viewmanagercpp)
- [Artifact 3: Systems Architecture and Resource Management](#artifact-3-systems-architecture-and-resource-management-shadermanagercpp)
- [Conclusion](#conclusion)

---

## Artifact 1: Software Engineering and Design (Backend Services & Testing)

* **Origin:** CS 320 (Software Engineering and Design)
* **Description:** A robust Java backend service designed to manage contacts, tasks, and appointments through clean object-oriented service classes, backed by a comprehensive JUnit test suite.

### The Problem
When I first wrote this application, the core logic worked fine for a classroom setting, but looking at it through a professional lens, it lacked the robust defensive programming required for production environments. The service layer trusted incoming parameters a bit too implicitly, meaning edge cases like null inputs, strings exceeding expected length limits, or attempts to modify or delete IDs that did not exist could lead to unhandled runtime exceptions or unstable states.

### The Enhancements
* **Strict Defensive Validation:** Implemented rigorous input validation and explicit error checking across all service classes to catch invalid states immediately.
* **Safe Deletion & State Handling:** Added explicit checks in methods like appointment and contact removal to verify that target IDs exist before modifying internal maps.
* **Expanded JUnit Testing:** Expanded test coverage far beyond the happy path, writing rigorous negative test cases to prove that every invalid input scenario correctly triggers expected exceptions like `IllegalArgumentException`.

---

## Artifact 2: Computational Graphics and Performance Optimization (`ViewManager.cpp`)

* **Origin:** CS 330 (Computational Graphics and Visualization)
* **Description:** A C++ rendering module responsible for managing camera perspectives, window scaling, and the transformation and projection pipeline for rendering 3D graphical spaces.

### The Problem
When I audited the original code, I noticed a severe performance bottleneck common in real-time rendering loops: every single frame tick, the application was blindly recalculating expensive perspective and orthographic projection matrices from scratch. Even if the window size hadn't changed by a single pixel and the camera view was completely static, the CPU was constantly wasting valuable cycles re-running heavy mathematical matrix calculations over and over again.

### The Enhancements
* **Intelligent State Caching:** Designed and implemented a state-caching mechanism that tracks state triggers, such as window resize events or camera zoom adjustments.
* **Reduced Per-Frame Overhead:** The application now checks whether a genuine change has occurred before recalculating the projection matrix. For all subsequent unchanged frames, it simply reuses the cached matrix, significantly cutting down on redundant mathematical overhead and improving runtime efficiency.

---

## Artifact 3: Systems Architecture and Resource Management (`ShaderManager.cpp`)

* **Origin:** CS 330 (Computational Graphics and Visualization)
* **Description:** Low-level C++ asset loading and shader management files (`MainCode.cpp` and `ShaderManager.h/cpp`) handling direct communication with the graphics processing unit via OpenGL.

### The Problem
In the original version of this application, everything compiled and rendered 3D scenes correctly, but it had a silent flaw under the hood regarding memory management: it relied entirely on the operating system to sweep up and reclaim allocated graphics shader program handles when the entire process finally terminated. In complex systems, leaving resource cleanup up to chance or process termination is a recipe for memory leaks and resource exhaustion.

### The Enhancements
* **RAII Principles (Resource Acquisition Is Initialization):** Refactored the architecture to incorporate modern C++ RAII patterns.
* **Explicit Destructors:** Added a custom destructor (`~ShaderManager()`) to safely check and invoke `glDeleteProgram(m_programID)` whenever a shader manager instance goes out of scope or is deleted, ensuring video memory handles are released deterministically the moment they are no longer needed.

---

## Conclusion

Bringing all three of these projects together into this final ePortfolio has been an incredible way to see how much my skills have grown across the entire spectrum of computer science. Whether it is adding strict defensive validation to backend Java services, cutting out redundant mathematical overhead in real-time C++ rendering loops, or managing low-level GPU memory safely with RAII patterns, these enhancements transformed my academic projects into polished, professional-grade software.
