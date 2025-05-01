Here's a **`docker-compose.yml`** file that:  
✅ Runs **code-server** with persistent configs  
✅ Binds a local project folder (`./project`)  
✅ Pre-installs **Java, Node.js, Python, Go, .NET, React, Angular**  
✅ Includes essential VS Code extensions  

---

### **`docker-compose.yml`**
```yaml
version: '3.8'

services:
  code-server:
    image: codercom/code-server:latest
    container_name: code-server
    restart: unless-stopped
    ports:
      - "8080:8080"
    volumes:
      - "$HOME/.config/code-server:/home/coder/.config/code-server"  # Persist VS Code configs
      - "./project:/home/coder/project"  # Mount local project folder
    environment:
      - PASSWORD=dev123  # Set a password (or use `--auth none` for no password)
      - DOCKER_USER=$USER
    # Custom setup: Install languages & tools
    command: >
      sh -c "
      # Install dependencies
      apt-get update && apt-get install -y \
          openjdk-17-jdk \
          python3 python3-pip \
          nodejs npm \
          golang \
          dotnet-sdk-6.0 \
          && npm install -g yarn @angular/cli create-react-app

      # Start code-server with extensions pre-installed
      code-server \
          --bind-addr 0.0.0.0:8080 \
          --auth password \
          --install-extension redhat.java \           # Java
          --install-extension ms-python.python \      # Python
          --install-extension ms-vscode.node-debug2 \  # Node.js
          --install-extension golang.go \              # Go
          --install-extension ms-dotnettools.csharp \ # .NET
          --install-extension angular.ng-template \    # Angular
          --install-extension dsznajder.es7-react-js-snippets # React
      "
```

---

### **Key Features**
1. **Pre-Installed Tools**  
   - Java (`openjdk-17-jdk`)  
   - Python (`python3`, `pip`)  
   - Node.js (`nodejs`, `npm`, `yarn`)  
   - Go (`golang`)  
   - .NET (`dotnet-sdk-6.0`)  
   - React (`create-react-app`)  
   - Angular (`@angular/cli`)  

2. **VS Code Extensions**  
   - Auto-installs essential extensions for each language.  

3. **Persistent Storage**  
   - VS Code settings saved in `~/.config/code-server`.  
   - Project files mapped to `./project` on your host machine.  

4. **Password Protection**  
   - Default password: `dev123` (change it!).  

---

### **How to Use**
1. **Start the Container**  
   ```bash
   docker-compose up -d
   ```
2. **Access VS Code**  
   Open `http://localhost:8080` in your browser.  

3. **Stop the Container**  
   ```bash
   docker-compose down
   ```

---

### **Customizations**
- **Change Password**: Modify `PASSWORD` in `environment`.  
- **Add More Tools**: Extend the `apt-get install` or `npm install` commands.  
- **HTTPS Setup**: Add an Nginx/Caddy reverse proxy (see [this guide](#)).  

Would you like help with **Kubernetes** or **Docker Swarm** deployment? 🚀
