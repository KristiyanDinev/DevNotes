# Docker Volumes: Named Volume vs Bind Mount

When working with Docker, you often need to store data outside the container so it persists. There are two main ways to do this:

- **Named Volumes**
- **Bind Mounts**

This guide explains the difference between them in a simple way.

---

## Named Volume

A **named volume** is storage managed by Docker itself. You just give it a name (like `uploads`) and Docker takes care of the rest.

### Example:
```yaml
volumes:
  - uploads:/app/uploads

volumes:
  uploads:
```

Docker stores the data in a special internal folder `(like /var/lib/docker/volumes/...)`, and you usually don't interact with it directly from your **host machine**.

## Bind Mount

A **bind mount** maps a specific folder on your **host machine** to a folder inside the container. It's like saying *"use this exact folder for container files."*

### Example:

```yml
volumes:
  - ./uploads:/app/uploads
```

Now, anything saved in the container at `/app/uploads` is actually saved to `./uploads` on your computer.

## Comparison Table

| Feature           | Named Volume                          | Bind Mount                       |
| ----------------- | ------------------------------------- | -------------------------------- |
| Host Access       | Hard to access manually               | Easy to access and edit          |
| File Location     | Stored in Docker's internal directory | Stored where you choose          |
| Portability       | Great for Docker-based environments   | Depends on local file paths      |
| Permissions       | Managed by Docker                     | Your responsibility              |
| Use Case          | Best for production & portability     | Best for development & debugging |

## Summary

- Use **named volumes** when you want Docker to manage storage and keep things clean.

- Use **bind mounts** when you're developing and want to easily view or edit files on your host.

