# PipeWire on Ubuntu 20.10

## No guarantees

This was written from memory. I give no guarantees as to the reliability of this readme. But I have PipeWire active now and I think these instructions are MOSTLY it.

## Prerequisites

1. Insanity
2. A working install of Ubuntu 20.10 (I used Pop_OS)
3. Most (all?) of the PipeWire compiled packages from https://tracker.debian.org/pkg/pipewire for your arch
  * At time of writing, I used 0.3.24-3
4. Missing/broken Pipewire dependencies (via https://packages.debian.org/unstable/libspa-0.2-bluetooth)
  * [libopenaptx0_0.2.0-5](https://packages.debian.org/sid/libopenaptx0)
  * [libopenaptx-dev_0.2.0-5](https://packages.debian.org/sid/libopenaptx-dev)
  * [libsbc1_1.5-3](https://packages.debian.org/sid/libsbc1)
  * [libsbc-dev_1.5-3](https://packages.debian.org/sid/libsbc-dev)

## Install

As usual, force an install via `dpkg -i`. libspa-0.2 will end up in a broken state thanks to bad dependencies if you don't install the openaptx and sbc overrides.

## Enable PipeWire

This command should force-enable it for your own user:
`systemctl --user enable pipewire-pulse.service pipewire-pulse.socket`

This command should force-enable it for ALL USERS:
`sudo ln -s /usr/share/doc/pipewire/examples/systemd/user/* /etc/systemd/user`

However, the latter will be missing two socket wants:
```
sudo ln -s /usr/share/doc/pipewire/examples/systemd/user/pipewire-pulse.service /etc/systemd/user/default.target.wants/pipewire-pulse.service
sudo ln -s /usr/share/doc/pipewire/examples/systemd/user/pipewire-pulse.socket /etc/systemd/user/socket.target.wants/pipewire-pulse.socket
```

## Give pulseaudio the big stop

PulseAudio is surprisingly resilient. The shortest way to tell it off is to mask the service.

This is for your own user:
```
mkdir -p $HOME/.config/systemd/user
systemctl --user mask pulseaudio.socket
```

This is for ALL USERS:
```
sudo ln -s /dev/null /etc/systemd/user/pulseaudio.service
sudo ln -s /dev/null /etc/systemd/user/pulseaudio.socket
```

## Reboot and hope for the best

See [the first item](#no-guarantees).