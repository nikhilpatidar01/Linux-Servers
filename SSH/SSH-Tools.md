# SSH Client Tools

* Connect to a remote server as `root`

  ```bash
  ssh root@192.168.1.31
  ```

* Connect to another remote server as `root`

  ```bash
  ssh root@192.168.1.34
  ```

* Connect as `root` on a custom SSH port `2222`

  ```bash
  ssh -l root -p 2222 192.168.1.34
  ```

* Connect as `user` on a custom SSH port `2200`

  ```bash
  ssh -p 2200 user@192.168.1.50
  ```

* Connect using a private key file named `id_rsa_centos`

  ```bash
  ssh -i id_rsa_centos root@192.168.1.34
  ```

* Connect using an ECDSA private key stored in default SSH folder

  ```bash
  ssh -i ~/.ssh/id_ecdsa root@192.168.1.34
  ```

* Connect with verbose output (useful for debugging)

  ```bash
  ssh -v root@192.168.1.34
  ```

* Connect with maximum verbosity and specify private key

  ```bash
  ssh -vvv -i ~/.ssh/id_rsa root@192.168.1.34
  ```

* Run a remote command (`id`) on port `22`

  ```bash
  ssh root@192.168.1.34 -p 22 'id'
  ```

* Copy a file to remote server's root directory

  ```bash
  scp file.txt root@192.168.1.34:/root/
  ```

* Copy multiple files to remote user's home directory

  ```bash
  scp file1.txt file2.txt root@192.168.1.34:/home/root/
  ```

* Copy multiple specific files to web server directory

  ```bash
  scp image.png notes.md root@192.168.1.100:/var/www/html/
  ```

* Copy an entire directory to a remote location

  ```bash
  scp -r /local/dir root@192.168.1.34:/remote/dir
  ```

* Copy a local Downloads folder to remote `/tmp` directory

  ```bash
  scp -r ~/Downloads root@192.168.1.34:/tmp/
  ```