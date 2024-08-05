## Description
People keep trying to trick my players with imitation flags. I want to make sure they get the real thing! I'm going to provide the SHA-256 hash and a decrypt script to help you know that my flags are legitimate. You can download the challenge files here:

    challenge.zip

The same files are accessible via SSH here: ssh -p 56794 ctf-player@rhea.picoctf.net Using the password 6abf4a82. Accept the fingerprint with yes, and ls once connected to begin. Remember, in a shell, passwords are hidden!

    Checksum: b09c99c555e2b39a7e97849181e8996bc6a62501f0149c32447d8e65e205d6d2
    To decrypt the file once you've verified the hash, run ./decrypt.sh files/<file>.

## Solution

When you unzip `challenge.zip`, you will find 3 files: checksum.txt, files directory and decrypt.sh. You can also access the files by SSH-ing into the provided webshell and navigating to : `cd home/ctf-player/drop-in`
First, we need to find which file inside files directory gives the checksum that matches `b09c99c555e2b39a7e97849181e8996bc6a62501f0149c32447d8e65e205d6d2`

For this, run `sha256sum files/* | grep b09c99c555e2b39a7e97849181e8996bc6a62501f0149c32447d8e65e205d6d2`. 

*Shell:*

```
> ctf-player@pico-chall$ sha256sum files/* | grep b09c99c555e2b39a7e97849181e8996bc6a62501f0149c32447d8e65e205d6d2
b09c99c555e2b39a7e97849181e8996bc6a62501f0149c32447d8e65e205d6d2  files/451fd69b
```

Now that we found out `files/451fd69b` matches the given checksum, we can decrypt the file using `decrypt.sh` to get the Flag

*Shell:*

```
> ctf-player@pico-chall$ decrypt.sh files/451fd69b
picoCTF{trust_but_verify_451fd69b}
```

## Flag
`picoCTF{trust_but_verify_451fd69b}`
