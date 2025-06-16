# 📦 1. Adapter Pattern

### ✅ Definition
- Adapter pattern allows incompatible interfaces to work together by converting one interface into another that the client expects.

### 🔧 Use Cases
- Integrating new components into legacy systems.  
- Adapting third-party libraries.  
- Bridging between incompatible APIs.  

### 📄 Example
    ****// Target interface
    interface MediaPlayer {
    void play(String audioType, String fileName);
    }
    
    // Adaptee class
    class VLCPlayer {
    void playVLC(String fileName) {
    System.out.println("Playing VLC: " + fileName);
    }
    }****

    // Adapter class
    class MediaAdapter implements MediaPlayer {
    VLCPlayer vlcPlayer = new VLCPlayer();

    public void play(String audioType, String fileName) {
        if ("vlc".equalsIgnoreCase(audioType)) {
            vlcPlayer.playVLC(fileName);
        }
    }
}

    // Client
    class AudioPlayer implements MediaPlayer {
    MediaAdapter adapter;

    public void play(String audioType, String fileName) {
        if ("vlc".equalsIgnoreCase(audioType)) {
            adapter = new MediaAdapter();
            adapter.play(audioType, fileName);
        } else {
            System.out.println("Playing MP3: " + fileName);
        }
    }
}
