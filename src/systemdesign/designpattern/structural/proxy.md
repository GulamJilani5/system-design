# 🧿 4. Proxy Pattern

### ✅ Definition
- Proxy pattern provides a surrogate or placeholder for another object to control access to it.

### 🔧 Use Cases
- Access control.    
- Lazy initialization (e.g., loading images/files).  
- Remote proxies (e.g., RMI).  
- Caching.  

### 📄 Example

        // Subject interface
        interface Image {
        void display();
        }
        
        // Real Subject
        class RealImage implements Image {
        private String filename;
        RealImage(String filename) {
        this.filename = filename;
        loadFromDisk();
        }

    private void loadFromDisk() {
        System.out.println("Loading " + filename);
    }

    public void display() {
        System.out.println("Displaying " + filename);
    }
    }

    // Proxy
    class ProxyImage implements Image {
    private RealImage realImage;
    private String filename;

    ProxyImage(String filename) {
        this.filename = filename;
    }

    public void display() {
        if (realImage == null) {
            realImage = new RealImage(filename);
        }
        realImage.display();
    }
}
