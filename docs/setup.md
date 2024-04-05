# Setup

## Board setup

1. screw heat sinks onto the RK1 compute modules

2. put compute modules onto the board

3. connect the power to the board

4. connect power supply to the board

5. observe that diodes are blinking

  The magic is happening!

6. discover the IP address of Turing Pi within my router

7. use `tpi` cli to connect to the board

  And fail because it is expecting HTTPS. Even when using `--host` flag.

8. prepare the SD card for flashing the firmware

9. download the lastest firmware

  https://firmware.turingpi.com/turing-pi2/

10. follow the instructions to flash the SD card with latest v2.x firmware

  Upgrading v1.x to v2.x: https://docs.turingpi.com/docs/turing-pi2-bmc-v1x-to-v2x

11. find the SD card using terminal on my Macbook

    macOS does not have `lsblk`, use `diskutil list` instead.

    In my case the SD card was `/dev/disk4`.

    **Make sure that the disk size matches the size of the SD card. Do not flash your own hard driver.**

12. unmount the SD card

    ```shell
    sudo diskutil unmountDisk /dev/${DISK}

    # Expected output:
    # Unmount of all volumes on ${DISK} was successful
    ```

13. flash the firmware to the SD card

    ```shell
    ls ~/Downloads

    # Make your life easier:

    export FIRMWARE=$HOME/Downloads/tp2-firmware-sdcard-v2.0.5.img
    # or any other firmware version you downloaded

    # To find the disk, run `sudo diskutil list`, or see above
    export DISK=/dev/disk4

    sudo dd if=$FIRMWARE of=$DISK bs=1M

    # Example output:
    # 54+1 records in
    # 54+1 records out
    # 57380864 bytes transferred in 8.416919 secs (6817324 bytes/sec)
    ```

    After the `dd` finished, you can disconnect the SD card.

14. power down the board

15. insert the SD card to the board on the back side

16. power on the board

17. the board should be flashing all four LAN LEDs to indicate that the firmware upgrade is ready

18. press the `KEY1` button on the board rapidly three times

19. the four LAN LEDs should start blinking like a progress bar (from left to right)

20. after the four LAN LEDs start double-blinking, the update is finished

21. power down the board

22. remove the SD card

23. power on the board

24. check the firmware :)

## Node setup

1. download the OS image from Turing Pi website

    https://firmware.turingpi.com/turing-rk1/

2. extract the `.img` file (if necessary)

3. open Turing Pi web ui and flash the OS

    1. select node
    2. select image file
    3. click "INSTALL OS"

    or use command line

    ```shell
    tpi flash --image-path $IMAGE --node $NODE

    # Example:
    tpi flash --image-path assets/ubuntu-22.04.3-preinstalled-server-arm64-turing-rk1_v1.33.img --node 4
    ```

4. wait for the transfer to complete (this will take some time)

5. repeat for all the nodes you want

6. check the IP address of node on my router

7. ssh into the machine

    ```shell
    ssh ubuntu@192.168.0.x
    # password: ubuntu - default, changed after login
    ```

8. update the Ubuntu

    ```shell
    sudo apt update
    sudo apt upgrade -y
    ```

9. add SSH public key to `~/.ssh/authorized_keys`

10. install Tailscale VPN

    see: https://tailscale.com/download/linux/ubuntu-2204

## Storage setup

1. power down the board

2. turn the board around

3. connect all four NVMe SSDs and screw them in

4. power on

5. ssh into each instance

6. validate that the disk is visible and accessible from RK1 OS (`lsblk`)
