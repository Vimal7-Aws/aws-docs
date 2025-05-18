# Mastering Dockerfile Instructions: CMD vs ENTRYPOINT and COPY vs ADD

Mastering Dockerfile instructions is essential for creating efficient, secure, and maintainable container images. In this post, we’ll break down two commonly misunderstood pairs of instructions—`CMD` vs `ENTRYPOINT` and `COPY` vs `ADD`—through a clear, side-by-side comparison.
If you find it helpful, your support will help continue the series!


---


## ↻ CMD vs ENTRYPOINT in Dockerfile

Both `CMD` and `ENTRYPOINT` define the command that runs when a container starts, but they behave differently.

## In CMD instructions the command line parameters passed will replace entirely where as in ENTRYPOINT the command line arguements will be appended
## CMD and ENTRYPOINT will be jointly used when you have to set some default parameters

---

## CMD

`CMD` defines the default command or arguments that are executed when a container starts—only if no other command is provided at runtime.

You can override `CMD` during container execution by passing a new command to `docker run`.

```dockerfile
FROM alpine
CMD ["echo", "Hello Readers"]
````

If you execute the container with `docker run image-name`, it will print:

```
Hello Readers
```

However, if you provide a different command when running the container, it overrides the `CMD` instruction entirely.
For example:

```bash
docker run image-name echo "Hello Devops Diaries Reader!"
```

This will produce the output:

```
Hello Devops Diaries Reader!
```

This behavior makes `CMD` useful when you want to define a default command, but still allow users or scripts to run something else if needed.

**Tip:** Use `CMD` when you want to specify default arguments or commands that can be easily changed by the user at runtime. For example, you might use `CMD` to set a default shell or script to execute.

## ENTRYPOINT

The `ENTRYPOINT` instruction defines a command that always executes when the container starts.
Unlike `CMD`, it cannot be overridden by a command passed at runtime unless you explicitly use the `--entrypoint` flag. If a command is provided during container execution, it is appended to the `ENTRYPOINT`.

```dockerfile
FROM alpine
ENTRYPOINT ["echo", "Hello Readers"]
```

Output:

```
Hello Readers
```

If we try to override the command by using the same as `CMD` in the runtime command

```bash
docker run image-name hello
```

Output:

```
Hello Readers hello
```

In this case, the command gets appended to the `ENTRYPOINT`.

## Example with --entrypoint to override the ENTRYPOINT

You can override the `ENTRYPOINT` at runtime using the `--entrypoint` flag. This allows you to change the command that is executed when the container starts.

```bash
docker run --entrypoint echo myimage "Hello, Custom Command"
```

In this case, instead of executing the default `ENTRYPOINT` (which is `echo Hello Readers`), it will execute `echo Hello, Custom Command`.

## ➕ COPY vs ADD in Dockerfile

Both `COPY` and `ADD` move files into a Docker image — but behave differently.

## COPY – Clean and Predictable

**Use it for:** Copying local files and folders from your build context.

**Syntax:**

```dockerfile
COPY <src> <dest>
```

**Why use it:**

  * Only handles local content
  * No extraction or downloading
  * Clear and secure

**Example:**

```dockerfile
FROM python:3.9
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY app.py .
```

## ADD – More Features, More Risk

**Use it for:** Archives and remote URLs.

**Syntax:**

```dockerfile
ADD <src> <dest>
```

**Extra features:**

  * Extracts `.tar`, `.gz`, etc., automatically to the target folder.
  * Downloads from URLs directly into the image.

When you use `ADD` with archives like `.tar` or `.gz`, it will automatically extract the contents to the specified target folder. This behavior can save you a step, but it also introduces complexity if you're not expecting it.

**Examples:**

**Extract an archive:**

```dockerfile
ADD app.tar.gz .
```

In this example, the contents of `app.tar.gz` will be automatically extracted into the current working directory (`.`). If you specify a folder, the contents will be extracted to that directory.

**Download from a URL:**

```dockerfile
ADD [https://example.com/file.txt](https://example.com/file.txt) /data/
```

This command downloads the `file.txt` from the URL and places it into the `/data/` directory inside the Docker image.

## Best Practice

  * Use `COPY` by default
  * Use `ADD` only for extraction or remote URLs (prefer `curl` or `wget` instead)

<!-- end list -->

```
```
