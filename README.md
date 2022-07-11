ROS Chat
=========

ROS Chat is a _very_ primitive text-based chat application built using ROS.  Channels and ROS topics are related 1:1.


roschat_msgs
-------------

ROS chat uses custom messages for sending messages on its channels.  These messages are defined in the `roschat_msgs`
package, allowing them to be built and deployed separately from the `roschat_server` and `roschat_client` packages.


roschat_server
---------------

The server is largely optional, but serves as a convenient central list of available channels.  The server should be
run on the same PC as the `roscore` process:
```
roslaunch roschat_server server.launch
```

The server's channels are loaded from a yaml file which contains the list of channels:
```
channels:
  - general
  - development
```

To add additional channels/topics simply add items to this array.

Every channel has 2 topics associated with it: `/roschat/{channel}` and `/roschat/{channel}/lurk`.  The main `{channel}`
topic is used for sending/receiving messages from `roschat_client` (see below).  The `lurk` topic is a `std_msgs/String`
republication of the messages received in order.  It is mainly used for debugging, but can also be used for read-only
access to the channel by users who do not have the `roschat_msgs` package built.


roschat_client
---------------

The ROS Chat client is a very primitive `ncurses` UI written in Python.
```
+++++++++++++++++++++++++++++++++++++++++++++  ROS Chat | chrisib | general  +++++++++++++++++++++++++++++++++++++++++++

11:12:18 beverlyh: good morning everyone
11:12:24 adamc: good morning bev
11:12:26 chrisib: hi
11:12:32 chrisib: hope you had a good weekend
11:12:38 beverlyh: thanks
11:12:40 adamc: tyvm
------------------------------------------------------------------------------------------------------------------------
?>

```

Each client is specific to a single channel.  If you want to communicate on multiple channels you must launch
multiple clients:
```
export ROS_MASTER_URI=http://<roschat_server_name>:11311
roslaunch roschat_client client.launch name:=$(whoami) channel:=general
```


Future Features
----------------

- add a channel browser to the `roschat_client`
- fix some rendering bugs in the client; sometimes the curses UI seems to get mangled
- add scrolling to the chat log so you can see older messages
