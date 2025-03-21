#raylib #C #async #gamedev

---
Loading resources asynchronously in Raylib is a bit tricky. It isn't something that is natively supported (yet). But in the mean time, we need a way to load resources in a non-blocking manner. Raylib uses an GLFW OpenGL backend, meaning that we can implement this behavior.

> [!Warning]
> This only works with a GLFW Raylib backend. If you're using something else, you're more or less out of luck.
# Concept:
1. Create a child GLFW context
2. Create a loading thread
3. Set the GLFW context on the loading thread to be our child context
4. Load the asset(s)
5. Close the child GLFW context (resources are maintained)
# Demonstration:
This video shows the async delayed loading implementation shown in [[Raylib Async Loading#Implementation]]. It shows the Raylib app launch, display it's framerate, and then display the loaded texture. It takes a few seconds, given the texture is 25MB large. You can see by the fact that the framerate counter continues (albeit more slowly) that the main thread is not being blocked.
# Implementation:
(uses [[Dialect]])
```C
#include "GLFW/glfw3.h"
#include "raylib.h"
#include <pthread.h>

// Struct for passing to our thread
typedef struct tex_load_ctx_t {
    char *path;
    Texture2D *out_tex;
    _Atomic u8 *out_loaded;
    GLFWwindow * glfw_window;
};

// This will be called on a thread
u0 _internal_tex_load(void *raw_ptr) {
    struct tex_load_ctx_t *ctx = (struct tex_load_ctx_t *) raw_ptr;

    // Set our current context to our loading context
    glfwMakeContextCurrent(ctx->glfw_window);

    // Actually load the texture
    *ctx->out_tex = LoadTexture(ctx->path);
    GenTextureMipmaps(ctx->out_tex);

    // Destroy the tex_load_wind
    glfwDestroyWindow(ctx->glfw_window);

	// Set the flag as loaded
    *ctx->out_loaded = 1;

    free(ctx);
}

u0 async_tex_load(char *path, Texture2D *out_tex, _Atomic u8 *out_loaded) {
    // We want the window we create to be 1x1 and invisible
    glfwWindowHint(GLFW_VISIBLE, GLFW_FALSE);

	// Malloc our struct and assign the fields
    struct tex_load_ctx_t *ctx = malloc(sizeof(struct tex_load_ctx_t));
    ctx->path = path;
    ctx->out_tex = out_tex;
    ctx->out_loaded = out_loaded;

	// Create a new glfw context where the loading will be done, it needs
	// to be based on our main application context. If you are handling
	// multiple glfw contexts, set the paramater instead of getting the current
    ctx->glfw_window = glfwCreateWindow(1, 1, "TexLoad", NULL, glfwGetCurrentContext());

    // Create our new thread and start it
    pthread_t thr;
    pthread_create(&thr, NULL, _internal_tex_load, (void *) ctx);
    pthread_detach(thr);
}

// Example main
int main(void) {
    InitWindow(640, 480, "Async loading example");
    SetTargetFPS(60);

    Texture2D texture = {0};
    _Atomic u8 loaded = 0;
    async_tex_load(ASSETS_PATH"example.png", &texture, &loaded);

	while(!WindowShouldClose()){
		BeginDrawing();
		ClearBackground(BLACK);

		// Check if the texture has been loaded
		if(loaded){
			DrawTexture(texture, 0, 0, WHITE);
		}

		EndDrawing();
	}
	CloseWindow();
	return 0;
}
```

