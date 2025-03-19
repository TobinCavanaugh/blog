Loading resources asynchronously in Raylib is a bit tricky. It isn't something that is natively supported (yet 😉). But in the mean time, we need a way to load resources in a non-blocking manner. Raylib uses an GLFW OpenGL backend, meaning that we should be able to implement this behavior.

### Concept:
1. Create a child GLFW context
2. Create a loading thread
3. Set the GLFW context on the loading thread to be our child context
4. Load the asset(s)
5. Close the child GLFW context (resources are maintained)

### Implementation
