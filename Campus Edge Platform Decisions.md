## 1: Target OS
> Should we build for Linux, Windows, or both?

Since this is a college campus project, the practical answer is:

- Primary runtime: Windows (because most student laptops use it).
    
- Learning environment: Linux (WSL on Windows or a Linux VM).
    
- Architecture: Cross-platform from day one.
    

This gives us the best of both worlds.

![](data:image/svg+xml;charset=utf-8,%3Csvg%20font-family%3D%22-apple-system-body%2C%20ui-sans-serif%2C%20-apple-system%2C%20system-ui%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Helvetica%2C%20%26quot%3BApple%20Color%20Emoji%26quot%3B%2C%20Arial%2C%20sans-serif%2C%20%26quot%3BSegoe%20UI%20Emoji%26quot%3B%2C%20%26quot%3BSegoe%20UI%20Symbol%26quot%3B%22%20font-weight%3D%22400%22%20data-d-component%3D%22svg%22%20fill%3D%22currentColor%22%20height%3D%22260%22%20style%3D%22color%3Argb\(255%2C%20255%2C%20255\)%22%20viewBox%3D%220%200%20720%20260%22%20width%3D%22100%25%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Crect%20x%3D%22260%22%20y%3D%2220%22%20width%3D%22200%22%20height%3D%2250%22%20rx%3D%2210%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Ctext%20x%3D%22360%22%20y%3D%2250%22%20text-anchor%3D%22middle%22%20font-size%3D%2218%22%3ECampus%20Edge%3C%2Ftext%3E%3Crect%20x%3D%2260%22%20y%3D%22120%22%20width%3D%22220%22%20height%3D%2290%22%20rx%3D%2210%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Ctext%20x%3D%22170%22%20y%3D%22150%22%20text-anchor%3D%22middle%22%20font-size%3D%2218%22%3EWindows%20Node%3C%2Ftext%3E%3Ctext%20x%3D%22170%22%20y%3D%22175%22%20text-anchor%3D%22middle%22%20font-size%3D%2213%22%3EWindows%20APIs%3C%2Ftext%3E%3Crect%20x%3D%22440%22%20y%3D%22120%22%20width%3D%22220%22%20height%3D%2290%22%20rx%3D%2210%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Ctext%20x%3D%22550%22%20y%3D%22150%22%20text-anchor%3D%22middle%22%20font-size%3D%2218%22%3ELinux%20Node%3C%2Ftext%3E%3Ctext%20x%3D%22550%22%20y%3D%22175%22%20text-anchor%3D%22middle%22%20font-size%3D%2213%22%3E%2Fproc%20%26amp%3B%20Linux%20APIs%3C%2Ftext%3E%3Cpath%20d%3D%22M360%2070%20L170%20120%20M360%2070%20L550%20120%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%20fill%3D%22none%22%2F%3E%3C%2Fsvg%3E)

Notice that Campus Edge sits above the operating system. Each OS gets its own adapter.

# How can one Node Agent work on both?

This introduces a fundamental software engineering pattern called abstraction.

Imagine the Master asks every node:

> "Give me your CPU information."

The Master doesn't care whether the node is Windows or Linux.

So we create one common interface.

```
ResourceProvider
       │
 ┌─────┴─────┐
 │           │
Windows   Linux
Provider  Provider
```

Think of it like charging cables.

- Your phone expects electricity.
    
- The wall outlet differs by country.
    
- The adapter hides the difference.
    

Our Node Agent will eventually do the same.

| Common Feature | Windows implementation | Linux implementation |
| -------------- | ---------------------- | -------------------- |
| CPU info       | Windows API            | `/proc/cpuinfo`      |
| Memory         | Windows API            | `/proc/meminfo`      |
| Processes      | Windows API            | `/proc/<pid>`        |
| Disk           | Windows API            | `statfs()`           |

The Master never knows the difference.