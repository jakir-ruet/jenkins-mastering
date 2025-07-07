## Welcome to Shared Libraries

### Shared Libraries

In Jenkins allow you to reuse pipeline code across multiple Jenkinsfiles or pipeline jobs. They are ideal for keeping your pipeline logic modular, maintainable, and DRY (Don’t Repeat Yourself).

### Shared Libraries Pipeline

- Avoid duplicating pipeline code.
- Centralize logic (e.g., build, deploy, test steps).
- Maintain consistent behavior across jobs.
- Enable DevOps teams to define custom reusable steps or tools.

### Project Structure

```bash
(root)
├── vars/
│   └── myCustomStep.groovy      # Simple reusable steps (like functions)
├── src/
│   └── org/my-app/MyClass.groovy   # Advanced, namespaced Groovy classes
├── resources/                   # Templates, static files, etc.
│   └── config.json
└── README.md
```
