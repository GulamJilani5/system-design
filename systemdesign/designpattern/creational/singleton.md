🔵🟢🔴➡️⭕🟠🟦🟣🟥🟧✔️⏺️ ☑️ • ‣ → ⁕

# ⏺️ Singleton - Only one object allowed

- The Singleton Pattern ensures that a class has only one instance and provides a global point of access to it.

### ➡️ 1. Eager Initialization

- Instance is created at class loading.
- Not suitable if instance creation is heavy and might not be used.

      public class Singleton {
      private static final Singleton instance = new Singleton();

      private Singleton() {}

      public static Singleton getInstance() {
      return instance;
      }
      }

# ➡️ 2. Lazy Initialization(Not Thread-safe)

- 🔹 Instance is created only when needed.
- 🔹 Not thread-safe by default.

      public class Singleton {
      private static Singleton instance;

      private Singleton() {}

      public static Singleton getInstance() {
      if (instance == null) {
      instance = new Singleton();
      }
      return instance;
      }
      }

# ➡️ 3. Synchronized (Thread-safe Singleton )

- Thread-safe.
- Performance overhead due to synchronization.

        public class Singleton {
          private static Singleton instance;

        private Singleton() {}

        public static synchronized Singleton getInstance() {
            if (instance == null) {
                instance = new Singleton();
            }
            return instance;
        }
       }

# ➡️ 4. Double-Checked Locking (Mostly used in practice)

- Thread-safe.
- High performance — synchronization happens only on first initialization.
- Requires volatile to avoid memory consistency errors.

      public class Singleton {

      private static volatile Singleton instance;

        private Singleton() {}

        public static Singleton getInstance() {
            if (instance == null) { // First check (no locking)
                synchronized (Singleton.class) {
                    if (instance == null) { // Second check (with locking)
                        instance = new Singleton();
                    }
                }
            }
            return instance;
        }

  }
