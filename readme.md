# pw

pw generates secure passwords based on your specifications. By default, pw generates 256-bit passwords using random printable ASCII symbols.

## Dependencies

1. Python

## Installation

```
curl https://raw.githubusercontent.com/mrcsmcln/pw/master/pw > pw
chmod 755 pw
./pw
```

Incredibly simple, no? If you want to make things *even simpler*, follow these steps:

1. Make a home subdirectory called `bin`: `mkdir ~/bin`
2. Move pw to that subdirectory: `mv pw ~/bin`
3. Add that subdirectory to your shell's path: `echo 'export PATH=$PATH:~/bin' >> ~/.bashrc`

Assuming you're using bash, you should now be able to run `pw` from anywhere in your shell.

## Usage

By default, pw generates 256-bit passwords from random printable ASCII symbols. This is currently understood to be unguessable. See [here](https://en.wikipedia.org/wiki/Password_strength#Required_Bits_of_Entropy) and [here](https://www.schneier.com/crypto-gram/archives/1999/0215.html) for more information.

```
pw
>@W^#!:v"`}rdm{|6A)VCw0"-v%y&Sx2IGLeM"'
```

This will work for *most but not all* of your purposes. Since banks, airlines, and other organizations tend to live in the past, our passwords can't always be 39 random symbols like you see above. Some organizations have *restrictions* and *limts*. We can get around these arbitrary rules like so:

```
pw --remove-punc
IHW2CEWA6 biYSAN28xuDWPZ5YdVd8TfnhDGmQw4tyD
```

Well, what if our bank tells us that `!`, `@`, `#`, `$`, `%`, `^`, `&`, and `*` are okay, but any other punctuation isn't allowed. This is how we handle that:

```
pw --remove-punc --add-custom '!@#$%^&*'
EIvduVEh#d &7V5e^F&AQ4X%B2OTwSCqNT&3lwoGvk
```

Now let's say we've got an airline that will allow us to use any punctuation *except* `[`, `]`, `(`, and `)`. Dead simple:

```
pw --remove-custom '[]()'
&1-yY<C7x5vF' j$A?@%sKF?j1I+o1a`1CW}PGDG
```

What if we need to start from scratch and *add* symbols instead of removing them from a predetermined set? Just `empty` the set and change `remove` to `add`:

```
pw --empty --add-upper --add-lower --add-digit --add-custom '!@#$%^&*'
!NauPyzWq5tkyO7y%Ypkvoa6@b8X5#ij^*w61IVhlR
```

What if our enterprise software doesn't like us using 42-symbol-long passwords? Instead, they say our password must be 16 symbols or less. To fix this, we simply specify the size of the password:

```
pw --size 16
@j'<lE\;=`Jwh=R.
```

The keen eye will notice that the prior examples, which don't specify a size, generate passwords of differing size. That's because we're using as many symbols as it takes to get to at least 256 bits of entropy. You could have a very long password, but if it's the same letter repeated over and over, it technically has zero entropy and wouldn't be hard to guess at all. So that's why we use bits by default instead of size.

If you ever need to change the number of bits of entropy required, you can do so like this:

```
pw --bits 128
KlxmT`s+^+~p<&gY 52l
```

Additionally, if you want to generate more than one password, you can do that as well:

```
pw --number 5
J]G|$|QlAq3jhTl5+^Lf Q<\Y(R<Bp# D/AW,jn
"al#l5vh'3aAXo86QoLC#&YOUXxWW3Ec F\YXpW
Z(!tS_40(!ON|8uT1EF_1+av%YB=d)NS/r(N[@0
7f$hG/%<L\6Sl$w1NNV2W9k7;LcwI-VjY6u{NKw
M(!D53+HIOL'e,xT"D2SBf;tM}H%@hvb#}X3M,T
```

And, if you would like to see additional information you can opt in:

```
pw --verbose
Symbol entropy: 6.569856 bits
Password entropy: 256 bits
Symbol set size: 95 symbols
Password size: 39 symbols

')w]zEc8u4PI^CWJSYykMKpH@^&LQd!(orT!&s"
```

Importantly, all of these arguments can be abbreviated! For example:

```
pw -rp -ac '!@#$%^&*' -b 128 -v
Symbol entropy: 6.149747 bits
Password entropy: 129 bits
Symbol set size: 71 symbols
Password size: 21 symbols

Zu# OKiKPlS&yvjb*H#4A
```

To see additional symbol sets, like octal digits and URI unreserved symbols, or for general help, use this:

```
pw --help
```