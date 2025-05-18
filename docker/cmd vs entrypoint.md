In Docker, both `CMD` and `ENTRYPOINT` are instructions in a Dockerfile that define the default command to be executed when a container is started. However, they have distinct purposes and interact in specific ways.

**`CMD` (Command):**

  * **Purpose:** The `CMD` instruction specifies the **default command** and its **parameters** to be executed when the container starts.
  * **Behavior:**
      * If you provide a command when running the container using `docker run <image> <command>`, the specified command **overrides** the `CMD` instruction in the Dockerfile.
      * If no command is provided during `docker run`, the command specified in the `CMD` instruction is executed.
      * There can be only **one** `CMD` instruction in a Dockerfile. If you have multiple, only the last one will be executed.
  * **Syntax:** `CMD ["executable", "param1", "param2"]` (exec form, preferred) or `CMD command param1 param2` (shell form). The exec form avoids an extra shell process.
  * **Use Cases:**
      * Providing a default application to run (e.g., `CMD ["node", "app.js"]`).
      * Setting default parameters for the `ENTRYPOINT` (as arguments).

**`ENTRYPOINT`:**

  * **Purpose:** The `ENTRYPOINT` instruction specifies the **executable** that will be run when the container starts. It defines the **main process** of the container.
  * **Behavior:**
      * When a container is run, the command specified in `ENTRYPOINT` is **always executed**.
      * Arguments provided in the `docker run <image> <arguments>` command are **appended** to the `ENTRYPOINT` instruction as parameters.
      * You can override the `ENTRYPOINT` behavior during `docker run` using the `--entrypoint` flag.
      * There can be only **one** `ENTRYPOINT` instruction in a Dockerfile. If you have multiple, only the last one will be executed.
  * **Syntax:** `ENTRYPOINT ["executable", "param1", "param2"]` (exec form, preferred) or `ENTRYPOINT command param1 param2` (shell form). The exec form is generally recommended.
  * **Use Cases:**
      * Making the container behave like an executable where the image name is the command.
      * Setting up a consistent environment or running a wrapper script before the main application starts.

**Interaction between `CMD` and `ENTRYPOINT`:**

The most interesting behavior occurs when both `CMD` and `ENTRYPOINT` are used together in their **exec form** (`["executable", "param1"]`). In this scenario:

  * The executable specified in `ENTRYPOINT` is executed.
  * The elements of the `CMD` instruction are passed as **arguments** to the `ENTRYPOINT` executable.
  * Arguments provided during `docker run` will **override** the arguments defined in `CMD`.

**Example:**

```dockerfile
FROM ubuntu:latest

ENTRYPOINT ["/bin/echo", "Hello"]
CMD ["World"]
```

When you run this image:

  * `docker run my-image`: The container will execute `/bin/echo Hello World`. (`ENTRYPOINT` provides `/bin/echo Hello`, and `CMD` provides `World` as an argument).
  * `docker run my-image Universe`: The container will execute `/bin/echo Hello Universe`. The `Universe` argument from `docker run` overrides the `CMD` argument `World`.
  * `docker run --entrypoint /bin/ls my-image -l`: The container will execute `/bin/ls -l`. The `--entrypoint` flag overrides the `ENTRYPOINT` instruction.

**Choosing Between `CMD` and `ENTRYPOINT`:**

  * Use `CMD` when you want to provide a default command that can be easily overridden when the container is run. This is common for providing a default application or script.
  * Use `ENTRYPOINT` when you want to make your container behave like an executable with a fixed main process. Arguments passed to `docker run` then become arguments to this main process. This is useful for creating specialized containers that always run a specific tool.
  * Use both `ENTRYPOINT` and `CMD` together when you have a main executable defined in `ENTRYPOINT` and you want to provide default arguments for it in `CMD`, which can be overridden by user-provided arguments.

**Best Practices:**

  * Prefer the **exec form** (`["executable", "param1"]`) for both `CMD` and `ENTRYPOINT` to avoid the overhead of an extra shell process and potential issues with signal handling.
  * Keep your `ENTRYPOINT` simple and focused on the main execution.
  * Use `CMD` to provide sensible defaults that users can easily customize.
  * Document the expected behavior of your container, especially how `ENTRYPOINT` and `CMD` interact.

Understanding how `CMD` and `ENTRYPOINT` work together is crucial for building flexible and well-behaved Docker images. They allow you to define the default execution of your containers while still providing users with the ability to customize their behavior when running them.
