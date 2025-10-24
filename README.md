# Swethaimport json
import hashlib
import os

# File to store user data
USER_DB = "users.json"


def load_users():
    """Load user data from JSON file."""
    if not os.path.exists(USER_DB):
        return {}
    with open(USER_DB, "r") as f:
        return json.load(f)


def save_users(users):
    """Save user data to JSON file."""
    with open(USER_DB, "w") as f:
        json.dump(users, f, indent=4)


def hash_password(password):
    """Return SHA-256 hash of a password."""
    return hashlib.sha256(password.encode()).hexdigest()


def register():
    users = load_users()
    username = input("Enter new username: ").strip()

    if username in users:
        print("❌ Username already exists. Try logging in.")
        return

    password = input("Enter password: ").strip()
    confirm = input("Confirm password: ").strip()

    if password != confirm:
        print("❌ Passwords do not match.")
        return

    users[username] = hash_password(password)
    save_users(users)
    print("✅ Registration successful!")


def login():
    users = load_users()
    username = input("Enter username: ").strip()
    password = input("Enter password: ").strip()

    if username not in users:
        print("❌ User not found. Please register first.")
        return

    hashed_pw = hash_password(password)
    if users[username] == hashed_pw:
        print(f"✅ Login successful! Welcome, {username}.")
    else:
        print("❌ Incorrect password.")


def main():
    print("=== USER AUTHENTICATION SYSTEM ===")
    while True:
        print("\n1. Register")
        print("2. Login")
        print("3. Exit")

        choice = input("Choose an option (1-3): ").strip()

        if choice == "1":
            register()
        elif choice == "2":
            login()
        elif choice == "3":
            print("👋 Goodbye!")
            break
        else:
            print("❌ Invalid choice. Please try again.")


if __name__ == "__main__":
    main()
