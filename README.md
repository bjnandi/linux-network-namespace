## What is Network Namespace

A network namespace is a logical copy of the network stack from the host system. Network namespaces are useful for setting up containers or virtual environments. Each namespace has its own IP addresses, network interfaces, routing tables, and so forth.

### Namespace Create
In linux root namespace run following command

```
$ sudo ip netns add red
```

After run this command linux create a namespce name with red 

(./redns.jpg)
Let's create another namespace:

```
$ sudo ip netns add blue
```
After run this command linux create a namespce name with blue

Now it's time to show all namespace we created. Run following command to show namespace

```
$ sudo ip netns list
```
Now create a virtual ethernet cable pair for connecting namespaces

```
$ sudo ip link add veth-red type veth peer veth-blue
```

Show vetch link list

```
$ sudo ip link list
```

Now veth cable connet to namespace

```
$ sudo ip link set veth-red netns red
```

```
$ sudo ip link set veth-blue netns blue
```


We are almost done. Now assign ip address with in each namespace

```
$ sudo ip netns exec red ip addr add 192.168.10.1/24 dev veth-red
$ sudo ip netns exec red ip link set veth-red up

$ sudo ip netns exec blue ip addr add 192.168.10.2/24 dev veth-blue
$ sudo ip netns exec blue ip link set veth-blue up
```

Now ping from one namespace to another namespace.

```
$ sudo ip netns exec red ping 192.168.10.2

$ sudo ip netns exec blue ping 192.168.10.1
```

### More command network namespace

Show ip adress of namespace
```
$ sudo ip netns exec red ip address
```

Show ip link of namespace
```
$ sudo ip netns exec red ip link
```
Enter a single namespace

```
$ sudo ip netns exec red bash
```
