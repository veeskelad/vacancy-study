# Adapting lab commands to the user's platform

Tool installation commands and runtime quirks differ between macOS, Linux distros, and WSL. The skill detects the platform once (step 0 in SKILL.md) and tailors the commands in the labs accordingly.

## Supported platforms

| Code | Platform | Package manager |
|-----|-----------|-----------------|
| `darwin` | macOS (Intel/ARM) | `brew` |
| `linux-debian` | Debian/Ubuntu/Mint/Pop!_OS | `apt` |
| `linux-rhel` | RHEL/Fedora/CentOS/Rocky/Alma | `dnf` (or `yum` on older systems) |
| `linux-arch` | Arch/Manjaro/EndeavourOS | `pacman` |
| `wsl` | Windows Subsystem for Linux | Depends on the distro (usually apt) |

WSL is technically Linux, but it has its quirks:
- The network stack is different (especially for K8s/Docker bridge)
- Docker Desktop on Windows may forward into WSL
- systemd can be disabled by default (on WSL1) — `sudo systemctl` won't work

## Universal template in the lab README

Inside `labs/labNN-topic/README.md`, the `## Prerequisites` section should show commands for several platforms:

```markdown
## Prerequisites

Install the tools:

**macOS:**
```bash
brew install <tool1> <tool2>
```

**Linux (Debian/Ubuntu):**
```bash
sudo apt update && sudo apt install -y <tool1> <tool2>
```

**Linux (RHEL/Fedora):**
```bash
sudo dnf install -y <tool1> <tool2>
```
```

If the user's platform is known (step 0), you can include only their block plus optionally the others for future reference.

## Standard tools — table

| Tool | macOS | Debian/Ubuntu | RHEL/Fedora | Arch |
|-----|-------|---------------|-------------|------|
| Docker | Docker Desktop (gui) or `brew install --cask docker` | `apt install docker.io docker-compose-plugin` | `dnf install docker docker-compose-plugin` | `pacman -S docker docker-compose` |
| kubectl | `brew install kubectl` | `apt install kubectl` (after adding GCloud repo) | `dnf install kubectl` (after Kubernetes repo) | `pacman -S kubectl` |
| kind | `brew install kind` | `curl -Lo ./kind https://kind.sigs.k8s.io/dl/...` | same | `pacman -S kind` |
| Helm | `brew install helm` | `apt install helm` | `dnf install helm` | `pacman -S helm` |
| ansible | `brew install ansible` | `apt install ansible` | `dnf install ansible` | `pacman -S ansible` |
| terraform | `brew install terraform` | `apt install terraform` (after HashiCorp repo) | `dnf install terraform` | `pacman -S terraform` |
| trivy | `brew install trivy` | `apt install trivy` (after Aquasec repo) | `dnf install trivy` | AUR |
| dive | `brew install dive` | `apt install dive` (after wagoodman repo) | `dnf install dive` (COPR) | AUR |
| buildah | via VM/Podman machine | `apt install buildah` | `dnf install buildah` | `pacman -S buildah` |
| gitlab-runner | `brew install gitlab-runner` | `apt install gitlab-runner` (after GitLab repo) | `dnf install gitlab-runner` | AUR |
| jq | `brew install jq` | `apt install jq` | `dnf install jq` | `pacman -S jq` |
| ldap-utils | `brew install openldap` (contains ldapsearch/ldapadd) | `apt install ldap-utils` | `dnf install openldap-clients` | `pacman -S openldap` |

If a tool isn't in the distro's default repository — explain how to add it (e.g. `curl install.sh | sh`, repo instructions, or a link to a release tarball).

## Tool-specific notes

### Buildah on macOS

Doesn't run natively (requires Linux namespaces). Options:

**Option 1: Lima VM** (recommended):
```bash
brew install lima
limactl start template://default
limactl shell default
# inside the VM
sudo apt install buildah
```

**Option 2: Podman machine**:
```bash
brew install podman
podman machine init
podman machine start
podman machine ssh
# inside the VM
sudo dnf install buildah
```

**Option 3: Docker wrapper** (quick start):
```bash
alias buildah='docker run --rm --privileged -v $(pwd):/workdir -w /workdir quay.io/buildah/stable buildah'
buildah --version
```

### kind (Kubernetes in Docker)

Works on every platform via Docker. On macOS it runs through Docker Desktop. On WSL — either through Docker Desktop with WSL integration or native Docker inside WSL.

macOS caveat: ingress on kind needs extra port mappings in the cluster config:
```yaml
nodes:
- role: control-plane
  extraPortMappings:
  - containerPort: 80
    hostPort: 80
```

### systemd on WSL

WSL2 supports systemd once `[boot] systemd=true` is added to `/etc/wsl.conf`. On WSL1 it doesn't work.

If a lab requires systemd (a full FreeIPA install, systemctl) — on WSL1, suggest the user switch to WSL2 or use a Docker-based alternative.

### Docker Desktop vs native Docker

| Environment | Docker engine | Cases where the difference matters |
|-------|---------------|-------------------------|
| macOS Desktop | Linux VM under the hood | Bind mounts are slower (use cached/delegated) |
| Linux native | Straight into the kernel | File operations are fast |
| Windows Desktop + WSL2 | Inside the WSL2 VM | Volume mounts through /mnt/c are slow |

In labs with volume mounts of large files (npm install into node_modules) — mention cached/delegated on macOS:
```yaml
volumes:
  - ./src:/app/src:cached
```

### apt repo for k8s/docker/etc

On Debian/Ubuntu many tools require adding an external repo. The standard pattern:

```bash
# Example for kubectl
sudo apt update && sudo apt install -y apt-transport-https ca-certificates curl
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt update && sudo apt install -y kubectl
```

If a lab uses such a tool — include the full repo-setup snippet, don't assume it's already there.

## Recording the platform

After step 0 — if there's a filesystem, write the choice to a file:

```bash
echo "darwin" > <area>-labs/PLATFORM.md
# or
echo "linux-debian" > <area>-labs/PLATFORM.md
```

In future sessions — read the file and don't ask again. The user can update it manually if they've switched to a different machine.

## If the platform is mixed

It happens that the user is on macOS at home and on Linux at work. In that case:
- Ask which environment we're using for labs right now
- In the README, provide commands for both main platforms
- Don't try to support a "universal" path that breaks on both

## What if the user doesn't know their platform

Rare but possible (especially with juniors). Ask gently:

> "Which computer will you be running the labs on? Do you have a Mac or a Windows/Linux PC? If it's a Mac — is it M1/M2/M3 or Intel?"

ARM (Apple Silicon / Raspberry Pi) is worth knowing separately — some Docker images are amd64-only and need `--platform linux/amd64` or buildx.
