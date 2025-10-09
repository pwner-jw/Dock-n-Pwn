# Dock-n-Pwn: Containerized Active Directory Tools

A collection of essential Active Directory security assessment tools, each containerized in Docker for easy, clean, and dependency-free execution. This project is designed for cybersecurity researchers, students, and penetration testers who need to quickly deploy tools in lab environments like Hack The Box or TryHackMe.

---

## ✨ Key Advantages

The primary goal of this project is to leverage Docker to overcome the most common frustrations with security tooling.

* **Run Anywhere, Instantly:** Forget OS-specific installation guides. If Docker is running, you can execute these tools on Windows, macOS, and any Linux distribution (Arch, Ubuntu, etc.) with the exact same commands — no setup friction.
* **No More "Dependency Hell":** Each tool is isolated in its own container, so you'll never have to worry about conflicting Python versions or system libraries on your host machine again.
* **Keep Your System Clean:** All dependencies are managed inside the container. There's no need to install anything globally, keeping your personal machine pristine.

This approach is ideal for anyone preparing for certifications like **OSCP** or **CPTS**.

---

## 🚀 Getting Started

### Prerequisites

The only requirement is a working installation of Docker on your system.

* Install Docker: [https://docs.docker.com/engine/install/](https://docs.docker.com/engine/install/)

### Installation & Usage

The process is the same for every tool:

1. Navigate to the tool's directory.
2. Build the Docker image.
3. Run the container to use the tool.

Below are specific instructions for each tool.

---

## 💡 Pro Tip: Create Aliases for Easy Access

To make the tools feel like native commands, add aliases to your shell configuration file (e.g., `~/.bashrc`, `~/.zshrc`, or `~/.config/fish/config.fish`). These aliases mount your current working directory automatically, making it easy to use local wordlists and save output.

Add these lines to your shell's config file:

```bash
# Dock-n-Pwn Aliases
alias bloodyad='docker run --rm -it --network=host -v "$(pwd)":/data -w /data bloodyad'
alias kerbrute='docker run --rm -it --network=host -v "$(pwd)":/app -w /app kerbrute'
alias targetedkerberoast='docker run --rm -it --network=host -v "$(pwd)":/data -w /data t-kerberoast'
```

After adding them, reload your shell (for example `source ~/.bashrc`) and you can use the commands directly (e.g., `bloodyad --help`).

---

# Tools

## 🩸 bloodyAd

An Active Directory security assessment tool that can perform a wide range of attacks and enumeration tasks.

**1. Build the Image:**

```bash
cd bloodyAd
docker build -t bloodyad .
```

**2. Run the Tool:**

**With alias (recommended):**

```bash
# Example after setting up alias:
bloodyad -d <DOMAIN> -u <USER> -p <PASSWORD> --host <DC_IP> get user 'TargetUser'
```

**Without alias:**

```bash
# Example: Get information about a user
docker run -it --rm --network=host bloodyad -d <DOMAIN> -u <USER> -p <PASSWORD> --host <DC_IP> get user 'TargetUser'
```

---

## 🐐 kerbrute

A tool for bruteforcing and enumerating valid Active Directory accounts through Kerberos pre-authentication.

**1. Build the Image:**

```bash
cd kerbrute
docker build -t kerbrute .
```

**2. Run the Tool:**

**With alias (recommended):**

```bash
# Example after setting up alias (assuming userlist.txt is in your current directory):
kerbrute userenum --dc <DC_IP> -d <DOMAIN> userlist.txt
```

**Without alias:**

```bash
# Example: Enumerate users from a wordlist
docker run -it --rm --network=host -v "$(pwd)/userlist.txt":/app/userlist.txt kerbrute userenum --dc <DC_IP> -d <DOMAIN> userlist.txt
```

---

## 🔥 targetedKerberoast

A tool for requesting Kerberos service tickets (TGS) in order to crack their passwords offline.

**1. Build the Image:**

```bash
cd targetedKerberoast
docker build -t t-kerberoast .
```

**2. Run the Tool:**

**With alias (recommended):**

```bash
# Example after setting up alias:
targetedkerberoast -d <DOMAIN> -u <USER> -p <PASSWORD> --dc-ip <DC_IP> -t <TARGET_SPN>
```

**Without alias:**

```bash
# Example: Request a TGS for a specific user
docker run -it --rm --network=host t-kerberoast -d <DOMAIN> -u <USER> -p <PASSWORD> --dc-ip <DC_IP> -t <TARGET_SPN>
```

---

## ⚖️ Disclaimer

The tools and techniques provided in this repository are for **educational and research purposes only**. They are intended to be used in authorized lab environments and for ethical hacking. The author is not responsible for any misuse or damage. Always obtain proper authorization before assessing any target.

---

## Contributing

Pull requests, issues, and feature requests are welcome. If you add a tool, please include a minimal `Dockerfile`, the build instructions, and a short usage example.
