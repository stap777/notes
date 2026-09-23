
Networking(22:09:26)
## Question 1: Why both MAC and IP?

Your answer:

> MAC is permanent, IP changes depending on the network.

Verdict: Good start, but we're missing why both exist.

Think about the postal system.

- IP address = the destination city and street address.
    
- MAC address = the exact apartment door inside the local building.
    

The important distinction isn't "permanent vs changing." It's scope.

|MAC|IP|
|---|---|
|Used inside the local network|Used across multiple networks|
|Read by switches|Read by routers|
|Usually tied to the network interface|Assigned by the network|

Here's a question to keep in your head until Session 2:

> If a router only cares about IP addresses, how does the final packet reach the correct Wi-Fi card inside your room?

The answer involves ARP, which we'll build later.

## Question 2: Which interfaces should Campus Edge use?

Your answer:

> Wi-Fi for now.

That's the practical decision.

But as engineers, let's separate policy from mechanism.

Our startup logic shouldn't say:

```
Use Wi-Fi.
```

Instead it should say:

```
Choose the first interface that:
- is UP
- is not loopback
- has an IPv4 address
- is not obviously virtual (for MVP)
```

Why?

Because some students will plug into Ethernet.

Some labs may disable Wi-Fi.

The node should adapt automatically.

So our policy becomes:

> "Use the best usable interface."

That makes the software portable.

## Question 3: Offline behavior

This answer is exactly how production systems are designed.

Even without internet, the OS still knows:

- hostname
    
- interfaces
    
- MAC addresses
    
- loopback
    
- interface state
    

That means a node can still:

- identify itself,
    
- prepare configuration,
    
- and later join the cluster.
    

Think of this as boot-time diagnostics.

# java
# What is an `Enumeration`?

Forget networking for two minutes.

Imagine the college librarian gives you a stack of books, but there's a rule:

- You can't see the whole shelf at once.
    
- You can't jump to Book #5.
    
- You can only ask:
    
    - "Is there another book?"
        
    - "Give me the next book."
        
    

That's an `Enumeration`.

![](data:image/svg+xml;charset=utf-8,%3Csvg%20font-family%3D%22-apple-system-body%2C%20ui-sans-serif%2C%20-apple-system%2C%20system-ui%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Helvetica%2C%20%26quot%3BApple%20Color%20Emoji%26quot%3B%2C%20Arial%2C%20sans-serif%2C%20%26quot%3BSegoe%20UI%20Emoji%26quot%3B%2C%20%26quot%3BSegoe%20UI%20Symbol%26quot%3B%22%20font-weight%3D%22400%22%20data-d-component%3D%22svg%22%20fill%3D%22currentColor%22%20style%3D%22color%3Argb\(255%2C%20255%2C%20255\)%22%20viewBox%3D%220%200%20700%20180%22%20width%3D%22100%25%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Crect%20x%3D%2230%22%20y%3D%2250%22%20width%3D%22120%22%20height%3D%2270%22%20rx%3D%2210%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Ctext%20x%3D%2290%22%20y%3D%2290%22%20text-anchor%3D%22middle%22%20font-size%3D%2218%22%3EWi-Fi%3C%2Ftext%3E%3Crect%20x%3D%22180%22%20y%3D%2250%22%20width%3D%22120%22%20height%3D%2270%22%20rx%3D%2210%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Ctext%20x%3D%22240%22%20y%3D%2290%22%20text-anchor%3D%22middle%22%20font-size%3D%2218%22%3EEthernet%3C%2Ftext%3E%3Crect%20x%3D%22330%22%20y%3D%2250%22%20width%3D%22120%22%20height%3D%2270%22%20rx%3D%2210%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Ctext%20x%3D%22390%22%20y%3D%2290%22%20text-anchor%3D%22middle%22%20font-size%3D%2218%22%3EVPN%3C%2Ftext%3E%3Crect%20x%3D%22480%22%20y%3D%2250%22%20width%3D%22160%22%20height%3D%2270%22%20rx%3D%2210%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Ctext%20x%3D%22560%22%20y%3D%2290%22%20text-anchor%3D%22middle%22%20font-size%3D%2218%22%3ELoopback%3C%2Ftext%3E%3Cpolygon%20points%3D%2278%2C30%2090%2C18%20102%2C30%22%20fill%3D%22currentColor%22%2F%3E%3Ctext%20x%3D%2290%22%20y%3D%22150%22%20text-anchor%3D%22middle%22%20font-size%3D%2214%22%3EPointer%3C%2Ftext%3E%3C%2Fsvg%3E)

The operating system is saying:

> "I'll hand you one interface at a time."

It does not give Java a ready-made `List`.

An `Enumeration` can do exactly two things.

## 1. `hasMoreElements()`

```
interfaces.hasMoreElements();
```

Think of this as peeking at the shelf.

> "Is there another interface left?"

Example:

|Pointer Position|Result|
|---|---|
|Wi-Fi|`true`|
|Ethernet|`true`|
|Loopback|`true`|
|End|`false`|

Nothing is removed yet.

You're only checking.

## 2. `nextElement()`

```
interfaces.nextElement();
```

This is where something actually happens.

Suppose the pointer is here:

![](data:image/svg+xml;charset=utf-8,%3Csvg%20font-family%3D%22-apple-system-body%2C%20ui-sans-serif%2C%20-apple-system%2C%20system-ui%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Helvetica%2C%20%26quot%3BApple%20Color%20Emoji%26quot%3B%2C%20Arial%2C%20sans-serif%2C%20%26quot%3BSegoe%20UI%20Emoji%26quot%3B%2C%20%26quot%3BSegoe%20UI%20Symbol%26quot%3B%22%20font-weight%3D%22400%22%20data-d-component%3D%22svg%22%20fill%3D%22currentColor%22%20style%3D%22color%3Argb\(255%2C%20255%2C%20255\)%22%20viewBox%3D%220%200%20700%20180%22%20width%3D%22100%25%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Crect%20x%3D%2230%22%20y%3D%2250%22%20width%3D%22120%22%20height%3D%2270%22%20rx%3D%2210%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Ctext%20x%3D%2290%22%20y%3D%2290%22%20text-anchor%3D%22middle%22%20font-size%3D%2218%22%3EWi-Fi%3C%2Ftext%3E%3Crect%20x%3D%22180%22%20y%3D%2250%22%20width%3D%22120%22%20height%3D%2270%22%20rx%3D%2210%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Ctext%20x%3D%22240%22%20y%3D%2290%22%20text-anchor%3D%22middle%22%20font-size%3D%2218%22%3EEthernet%3C%2Ftext%3E%3Crect%20x%3D%22330%22%20y%3D%2250%22%20width%3D%22120%22%20height%3D%2270%22%20rx%3D%2210%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Ctext%20x%3D%22390%22%20y%3D%2290%22%20text-anchor%3D%22middle%22%20font-size%3D%2218%22%3EVPN%3C%2Ftext%3E%3Crect%20x%3D%22480%22%20y%3D%2250%22%20width%3D%22160%22%20height%3D%2270%22%20rx%3D%2210%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Ctext%20x%3D%22560%22%20y%3D%2290%22%20text-anchor%3D%22middle%22%20font-size%3D%2218%22%3ELoopback%3C%2Ftext%3E%3Cpolygon%20points%3D%2278%2C30%2090%2C18%20102%2C30%22%20fill%3D%22currentColor%22%2F%3E%3C%2Fsvg%3E)

Calling:

```
NetworkInterface ni = interfaces.nextElement();
```

does two things simultaneously.

### Before

| Pointer | Wi-Fi |

### After

- `ni` contains the Wi-Fi object.
    
- The pointer moves automatically to Ethernet.
    

Think of drawing a card from the top of a deck.

You don't manually move the next card.

The deck already knows where the next one is.

# What happens if you call `nextElement()` at the end?

Suppose there are only four interfaces.

You call:

```
nextElement();
nextElement();
nextElement();
nextElement();
```

Everything works.

Now you call it one more time.

There is no fifth interface.

Java throws:

```
NoSuchElementException
```

That's why this pattern always exists:

```
while (interfaces.hasMoreElements()) {
    interfaces.nextElement();
}
```

The check prevents us from reading past the end.

# How the Pointer Moves

|Step|Pointer Before|`nextElement()` Returns|Pointer After|
|---|---|---|---|
|1|Wi-Fi|Wi-Fi|Ethernet|
|2|Ethernet|Ethernet|VPN|
|3|VPN|VPN|Loopback|
|4|Loopback|Loopback|End|
|5|End|❌ Exception|End|

Notice something important.

Your earlier guess was:

> "Maybe it resets."

It doesn't.

An `Enumeration` is like a conveyor belt.

Once you've walked through it, it's exhausted.

If you want to start over, you must ask the operating system for a new Enumeration.

That design choice is exactly why we decided earlier to copy everything into a `List`.

# OS
## file
A file is a sequence of bytes stored by the operating system.

That's it.

The OS does not know whether the file contains:

- C code
    
- Java code
    
- A JPEG
    
- An MP3
    
- A PDF
    

It only stores bytes.

Think of the disk as a huge warehouse.

![](data:image/svg+xml;charset=utf-8,%3Csvg%20font-family%3D%22-apple-system-body%2C%20ui-sans-serif%2C%20-apple-system%2C%20system-ui%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Helvetica%2C%20%26quot%3BApple%20Color%20Emoji%26quot%3B%2C%20Arial%2C%20sans-serif%2C%20%26quot%3BSegoe%20UI%20Emoji%26quot%3B%2C%20%26quot%3BSegoe%20UI%20Symbol%26quot%3B%22%20font-weight%3D%22400%22%20data-d-component%3D%22svg%22%20fill%3D%22currentColor%22%20style%3D%22color%3Argb\(255%2C%20255%2C%20255\)%22%20viewBox%3D%220%200%20700%20220%22%20width%3D%22100%25%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Crect%20x%3D%2220%22%20y%3D%2240%22%20width%3D%22660%22%20height%3D%22120%22%20rx%3D%2212%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Crect%20x%3D%2240%22%20y%3D%2270%22%20width%3D%2270%22%20height%3D%2260%22%20fill%3D%22%23E5E7EB%22%2F%3E%3Crect%20x%3D%22120%22%20y%3D%2270%22%20width%3D%2270%22%20height%3D%2260%22%20fill%3D%22%23DBEAFE%22%2F%3E%3Crect%20x%3D%22200%22%20y%3D%2270%22%20width%3D%2270%22%20height%3D%2260%22%20fill%3D%22%23DCFCE7%22%2F%3E%3Crect%20x%3D%22280%22%20y%3D%2270%22%20width%3D%2270%22%20height%3D%2260%22%20fill%3D%22%23FDE68A%22%2F%3E%3Crect%20x%3D%22360%22%20y%3D%2270%22%20width%3D%2270%22%20height%3D%2260%22%20fill%3D%22%23FECACA%22%2F%3E%3Crect%20x%3D%22440%22%20y%3D%2270%22%20width%3D%2270%22%20height%3D%2260%22%20fill%3D%22%23E9D5FF%22%2F%3E%3Crect%20x%3D%22520%22%20y%3D%2270%22%20width%3D%2270%22%20height%3D%2260%22%20fill%3D%22%23E5E7EB%22%2F%3E%3Crect%20x%3D%22600%22%20y%3D%2270%22%20width%3D%2260%22%20height%3D%2260%22%20fill%3D%22%23DBEAFE%22%2F%3E%3Ctext%20x%3D%22350%22%20y%3D%2230%22%20text-anchor%3D%22middle%22%20font-size%3D%2218%22%3EStorage%3C%2Ftext%3E%3Ctext%20x%3D%2275%22%20y%3D%22150%22%20text-anchor%3D%22middle%22%20font-size%3D%2212%22%3E0x63%3C%2Ftext%3E%3Ctext%20x%3D%22155%22%20y%3D%22150%22%20text-anchor%3D%22middle%22%20font-size%3D%2212%22%3E0x6F%3C%2Ftext%3E%3Ctext%20x%3D%22235%22%20y%3D%22150%22%20text-anchor%3D%22middle%22%20font-size%3D%2212%22%3E0x75%3C%2Ftext%3E%3Ctext%20x%3D%22315%22%20y%3D%22150%22%20text-anchor%3D%22middle%22%20font-size%3D%2212%22%3E0x6E%3C%2Ftext%3E%3Ctext%20x%3D%22395%22%20y%3D%22150%22%20text-anchor%3D%22middle%22%20font-size%3D%2212%22%3E0x74%3C%2Ftext%3E%3C%2Fsvg%3E)

Each box stores one byte.

The OS simply says,

> "Here are the bytes you asked for."

Meaning comes later.

## What is a Byte?

Before we build a lexer that reads bytes, we need to know what a byte actually is.

A bit is the smallest piece of digital information.

It has only two possible values.

|Bit|Meaning|
|---|---|
|`0`|Off|
|`1`|On|

A byte is:

8 bits8\text{ bits}8 bits

Example byte:

011000110110001101100011

This is eight tiny electrical states.

Nothing more.