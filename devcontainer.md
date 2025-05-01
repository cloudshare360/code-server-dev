Here's an enhanced **`docker-compose.yml`** with **Dev Container support** (via `.devcontainer` config) to provide a fully preconfigured VS Code environment with:  

✅ **Pre-installed languages** (Java, Node.js, Python, Go, .NET, React, Angular)  
✅ **VS Code extensions** (auto-installed)  
✅ **Dev Container configuration** (for seamless VS Code Remote-Containers integration)  
✅ **Bind-mounted volumes** (for persistent data)  

---

### **File Structure**
```bash
your-project/
├── .devcontainer/
│   ├── devcontainer.json  # VS Code Dev Container config
│   └── Dockerfile         # Custom Docker image
└── docker-compose.yml     # Docker Compose setup
```

---

### **1. `.devcontainer/devcontainer.json`**
```json
{
  "name": "Code-Server Dev Container",
  "dockerComposeFile": "../docker-compose.yml",
  "service": "code-server",
  "workspaceFolder": "/home/coder/project",
  "shutdownAction": "stopCompose",
  "customizations": {
    "vscode": {
      "extensions": [
        "redhat.java",
        "ms-python.python",
        "ms-vscode.node-debug2",
        "golang.go",
        "ms-dotnettools.csharp",
        "angular.ng-template",
        "dsznajder.es7-react-js-snippets"
      ]
    }
  }
}
```

---

### **2. `.devcontainer/Dockerfile`**
```dockerfile
FROM codercom/code-server:latest

# Install system dependencies
RUN sudo apt-get update && sudo apt-get install -y \
    openjdk-17-jdk \
    python3 python3-pip \
    nodejs npm \
    golang \
    dotnet-sdk-6.0 \
    && sudo npm install -g yarn @angular/cli create-react-app

# Install VS Code extensions
RUN code-server --install-extension redhat.java \
    && code-server --install-extension ms-python.python \
    && code-server --install-extension ms-vscode.node-debug2 \
    && code-server --install-extension golang.go \
    && code-server --install-extension ms-dotnettools.csharp \
    && code-server --install-extension angular.ng-template \
    && code-server --install-extension dsznajder.es7-react-js-snippets

# Set default shell (optional)
ENV SHELL=/bin/bash
```

---

### **3. `docker-compose.yml`**
```yaml
version: '3.8'

services:
  code-server:
    build:
      context: .
      dockerfile: .devcontainer/Dockerfile
    container_name: code-server
    restart: unless-stopped
    ports:
      - "8080:8080"
    volumes:
      - "$HOME/.config/code-server:/home/coder/.config/code-server"
      - "./project:/home/coder/project"
    environment:
      - PASSWORD=dev123  # Change this!
      - DOCKER_USER=$USER
    command: code-server --bind-addr 0.0.0.0:8080 --auth password
```

---

### **Key Features**
1. **Dev Container Integration**  
   - VS Code detects `.devcontainer` and offers to **reopen in container**.  
   - Works with **VS Code Remote-Containers** extension.  

2. **Preconfigured Tools**  
   - Languages: Java, Python, Node.js, Go, .NET, React, Angular.  
   - Extensions auto-installed in both **Dev Container** and **code-server**.  

3. **Persistent Storage**  
   - VS Code configs saved in `~/.config/code-server`.  
   - Project files synced to `./project` on host.  

---

### **How to Use**
1. **Start the Dev Environment**  
   ```bash
   docker-compose up -d --build
   ```
   - Access **code-server** at `http://localhost:8080`.  
   - Or, open in VS Code with **Remote-Containers**.  

2. **VS Code Remote-Containers Workflow**  
   - Open the project in VS Code.  
   - Click **"Reopen in Container"** (bottom-left corner).  

3. **Stop the Environment**  
   ```bash
   docker-compose down
   ```

---

### **Customizations**
- **Change Password**: Update `PASSWORD` in `docker-compose.yml`.  
- **Add More Tools**: Extend the `Dockerfile` (e.g., Ruby, Rust).  
- **HTTPS**: Add an Nginx/Caddy reverse proxy.  

Would you like help with **debugging configurations** (e.g., `launch.json`) or **Kubernetes deployment**? 🚀
