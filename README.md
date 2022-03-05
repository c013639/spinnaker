Install on Debian/Ubuntu and macOS
Halyard runs on…

Ubuntu 14.04, 16.04 or 18.04 (Ubuntu 16.04 requires Spinnaker 1.6.0 or later)
Debian 8 or 9
macOS (tested on 10.13 High Sierra only)
Get the latest version of Halyard:

For Debian/Ubuntu:

curl -O https://raw.githubusercontent.com/spinnaker/halyard/master/install/debian/InstallHalyard.sh
For macOS:

curl -O https://raw.githubusercontent.com/spinnaker/halyard/master/install/macos/InstallHalyard.sh
Install it:

sudo bash InstallHalyard.sh

If you’re prompted for any information, default answers are usually suitable.

Check whether Halyard was installed properly:

hal -v

If this command fails, make sure hal is in your $PATH, and check the logs under /var/log/spinnaker/halyard/halyard.log.

Run . ~/.bashrc to enable command completion.

To get help with any hal command, append -h. Also, see the Halyard command Reference .

Update Halyard on Debian/Ubuntu or macOS
sudo update-halyard

Install Halyard on Docker
Note: If you install Halyard in a Docker container, you will need to manually change permissions on the mounted ~/.hal directory to ensure Halyard can read and write to it.

Make sure you have Docker CE installed .

On your current machine, make a local Halyard config directory.

mkdir ~/.hal
This will persist between runs of the Halyard docker container.

Start Halyard in a new Docker container.

The following command creates the Halyard Docker container, mounting the Halyard config directory:

docker run -p 8084:8084 -p 9000:9000 \
    --name halyard --rm \
    -v ~/.hal:/home/spinnaker/.hal \
    -it \
    us-docker.pkg.dev/spinnaker-community/docker/halyard:stable
This runs as a foreground process in your current shell. This is useful because it emits all of the Halyard daemon’s logs, which are not persisted. If you don’t care about the logs, and would rather run in detached mode, replace the -it with -d

Note: Any secrets/config you need to supply to the daemon (for example, a kubeconfig file) must be mounted in either your local ~/.hal directory, or another directory that you supply to docker run with additional -v command-line options.

In a separate shell, connect to Halyard:

docker exec -it halyard bash
You can interact with Halyard from here.

Run the following command to enable command completion:

source <(hal --print-bash-completion)
To get help with any hal command, append -h. Also, see the Halyard command Reference .

Update Halyard on Docker
Fetch the latest Halyard version.

docker pull us-docker.pkg.dev/spinnaker-community/docker/halyard:stable
Stop the running Halyard container.

docker stop halyard

Re-run the container:

docker run -p 8084:8084 -p 9000:9000 \
    --name halyard --rm \
    -v ~/.hal:/home/spinnaker/.hal \
    -it \
    us-docker.pkg.dev/spinnaker-community/docker/halyard:stable
This re-starts the container using the updated image you got in step 1.

In a separate shell, run:

docker exec -it halyard bash
Uninstall Halyard from Docker
To uninstall Halyard, just delete the container.

docker rm halyard