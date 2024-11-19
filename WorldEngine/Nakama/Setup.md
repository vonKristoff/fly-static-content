If you are using Windows, download and setup WSL.
## GitHub

### SSH key creation (optional)
> SSH keys allow you to access private repositories from the command line.

Optionally, in case one of the repositories will be changed to private, you need to have a SSH key setup. Run the following command to check if you already have existing SSH keys.
```shell
ls -al ~/.ssh
```
If you do not, run the following commands using your email address. This will move the file automatically in the right folder (`~/.ssh`). Make sure you set a secure passphrase as this SSH key has a large scope and could endanger your account if it get's in the wrong hands.
```shell
ssh-keygen -t ed25519 -C "your_email@example.com"
```
This command has generated a private key called `ed25519` without any file extensions and a public key called `ed25519.pub`.

Now install `gh` to link the key to your GitHub account. And then login to your account. Select the GitHub.com account option to login via the browser. 
```shell
sudo apt install gh
gh auth login
```
After you are logged in, add the private key (the one without a file extension) using the commands:
```shell
cd ~/.ssh
ssh-add ed25519
```

### Git config setup
Setup your `~/.gitconfig` file to include the following:
```toml
[user]
        name = YOUR USERNAME
        email = YOUR EMAIL
        
[url "git@github.com:ArchetypalTech/TheOrugginTrail-ArgusWE.git"]
insteadOf = [https://github.com/ArchetypalTech/TheOrugginTrail-ArgusWE.git](https://github.com/ArchetypalTech/TheOrugginTrail-ArgusWE.git)**
```

Next up, we need to clone the public backend repo for local development, so make a directory you want to clone the repo into and run
```shell
git clone git@github.com:ArchetypalTech/TheOrugginTrail-ArgusWE.git
```

Here are some general useful commands when working with git:
```shell
git clone #-> clones a repo.

git branch #-> indicates on which branch you currently are.

git checkout nameOfTheBranch #-> switches to the specified branch.

git add nameOfFile/Folder #-> adds the files or folders that want to be pushed.

git commit -m #-> commits the added files/folders. You will have to add a description after the -m flag (git commit -m “testing” - the -m flag does not refer to the main branch, it is for description)

git push #-> pushes the committed files/folders
```

## Cardinal
>Cardinal is a World Engine game shard framework for building powerful, highly scalable on-chain games.
>
>Through the World Engine’s shard communication interface (SCI), Cardinal interoperates seamlessly with EVM smart contracts on the World Engine stack, providing web2 scale performance for on-chain games while preserving interoperability.

### Cardinal Integration
#### Go Lang Installation
> World Engine (WE) is built using [Go](https://golang.org/).

First of all, check if you have Go installed. We are looking for version `1.22.4`. To check, run:
```shell
go version # if installed: go version go1.22.4 linux/amd64
```
If it's not installed, download the `go1.22.4.linux-amd64.tar.gz` version from the [Go website](https://golang.org/doc/install) and drag it into your users home directory using the Windows explorer `Linux/Ubuntu/home/USER`. Then run the following command to uninstall any old versions and unzip and install the new one.
```shell
rm -rf /usr/local/go && tar -C /usr/local -xzf go1.22.4.linux-amd64.tar.gz
```

If you are installing go in a VM that has Ubuntu, you need to use wget to download the tarball
```shell
wget https://golang.org/dl/go1.22.4.linux-amd64.tar.gz
```

Next you will have to extract the tarball
```shell
sudo tar -C /usr/local -xzf go1.22.4.linux-amd64.tar.gz
```

Next up, open your *.bashrc* file
```shell
nano ~/.bashrc
```
and paste the following line and save and exit nano with `Ctrl+X`.
```shell
export PATH=$PATH:/usr/local/go/bin
```
This will make the command `go` globally available. Run the following command to update bash to include the command `go`. You may need to restart WSL by closing and opening the Ubuntu window.
```shell
source ~/.bashrc
```
#### Docker Installation
> Docker is a containerization tool that allows you to run applications in a sandboxed environment; it ensures that the World Engine stack will run the same way on your machine as it will on production deployments.

Check if docker is installed via the command
```shell
docker --version # if installed: Docker version 26.1.4, build 5650f9b
```
If it's not installed, run
```shell
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt upgrade
sudo apt install docker-ce docker-ce-cli containerd.io
docker --version # verify docker is installed
```
Once the basics are installed, finish the docker setup with the following commands. You have to restart WSL again to re-evaluate the group changes.
```shell
sudo groupadd docker
sudo usermod -aG docker $USER
```
You can verify if docker is running by executing the command
```shell
sudo service docker status # Should return a view with a green *active (running)* indicator
```

#### World CLI Tool Installation
> [World CLI](https://github.com/argus-labs/world-cli) is a Swiss army knife command-line tool for creating, managing, and deploying World Engine projects.

Install the World CLI tool by running 
```shell
curl https://install.world.dev/cli@v1.2.8! | bash # We want version 1.2.8 as the latest version includes a bug.
```

### World Setup

// TODO temporary - no longer neccessary this step after Bernado's next commit.
Navigate to the directory where you installed `TheOrugginTrail-ArgusWE` and open up the docker file and change the `services.nakama.image` line to
```yml
    image: ghcr.io/argus-labs/world-engine-nakama:1.2.5
```
Then start the cardinal server with nakama using:
```shell
world cardinal start --editor
```
This command will allow you to access the WE editor tool to see in real time the systems running.

You can find out more about cardinal using the command flag `--help`:
```shell
world cardinal --help
```
Navigate to the frontend root directory and start it as well using:
```shell
npm run dev
```

By now
- Nakama should be running under `localhost:7350`
- Nakama console is running under `localhost:7351`
- Cardinal Editor under `localhost:3000`
- and the Svelte frontend under `localhost:5173`

Open up the frontend and wait for the console message that a persona was created. Then type *LOOK WITH BOTTLE AT THE WINDOW* and the server should respond.