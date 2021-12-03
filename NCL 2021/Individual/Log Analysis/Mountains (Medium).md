# Mountains (Medium)

## Flavor Text

We stumbled upon a box running a fairly different OS what can you tell us about it?

## What distribution is this box running?

```cat /etc/*release```

> Alpine

## What is the version number?

```cat /etc/*release```

> 3.14.2

## How many packages are installed?

```apk list -I | wc -l```

> 71


## What is the name of the custom virtual package?

```cat /etc/apk/world```

> liber8tion-dev=20211012.181645

## What package is a dependency of that virtual?

```apk info -a liber8tion-dev```

> ruby