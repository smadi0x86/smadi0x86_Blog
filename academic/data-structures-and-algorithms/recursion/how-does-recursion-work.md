---
cover: https://cdn.pixabay.com/photo/2022/06/12/22/48/gradient-7258997_960_720.png
coverY: 0
layout:
  cover:
    visible: true
    size: full
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# How Does Recursion Work?

To understand how recursion works, we need to look at the behavior of the run time stack as we make the function calls.

The runtime stack is a structure that keeps track of function calls and local variables as the program runs. When a program begins, the main() function is placed on the run time stack along with all variables local to main().&#x20;

Each time a function is called, it gets added to the top of the runtime stack along with variables and parameters local to that function. Variables below it become inaccessible. When a function returns, the function along with it's local variables are popped off the stack allowing access to its caller and its callers variables.

#### Suppose we have the following program:

```cpp
unsigned int factorial(unsigned int n){
    // base case result
    unsigned int rc = 1 ;              
     //if n > 1 we  have the recursive case
    if(n > 1)                
        rc= n * factorial(n-1);  // rc is n * (n-1)!
    }
    return rc;
}

int main(void){
    unsigned int x = factorial(4);
    
    return 0;
}
```

#### Lets trace what happens to it with respect to the run time stack:

Program begins, main() function is placed on stack along with local variable x.&#x20;

<figure><img src="https://catherine-leung.gitbook.io/~gitbook/image?url=https%3A%2F%2F2335496195-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LFrWzEqLSRHjU6HG9dw%252F-LGxd8qDaQ1QRuKAxM8i%252F-LGxfEKEXYdlI-dBkTA6%252Frecursion1.png%3Falt%3Dmedia%26token%3D30acfe78-8733-42d9-8f8b-d50da67952ce&#x26;width=300&#x26;dpr=4&#x26;quality=100&#x26;sign=71b8de107cae681f22397f5e7d89a1fc34cafe6f5e10feff4d723210fdca162f" alt=""><figcaption></figcaption></figure>

factorial(4) is called, so we push factorial(4) onto the stack along with local variables n and rc.

<figure><img src="https://catherine-leung.gitbook.io/~gitbook/image?url=https%3A%2F%2F2335496195-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LFrWzEqLSRHjU6HG9dw%252F-LGxd8qDaQ1QRuKAxM8i%252F-LGxfxunwjAikMPgiwhL%252Frecursion2.png%3Falt%3Dmedia%26token%3D5f294347-0004-4740-b998-6dde40dd23d4&#x26;width=300&#x26;dpr=4&#x26;quality=100&#x26;sign=9124588bc54c30db07c6dd89e615a44dae956d4a6f46eabd4401a5411203a6fa" alt=""><figcaption></figcaption></figure>

n is 4, and thus the if statement in line 3 is true, thus we need to call factorial(3) to complete line 4.&#x20;

<figure><img src="https://catherine-leung.gitbook.io/~gitbook/image?url=https%3A%2F%2F2335496195-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LFrWzEqLSRHjU6HG9dw%252F-LGxd8qDaQ1QRuKAxM8i%252F-LGxgLYmT77g1SSsJSuL%252Frecursion3.png%3Falt%3Dmedia%26token%3D2ca6df80-e2c7-4f00-9897-e6860c956e3a&#x26;width=300&#x26;dpr=4&#x26;quality=100&#x26;sign=e3335607097746743c3a522c24024ce6826401840dbcc7f3bfb81ff279e3321d" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Note that in each function call to factorial on stack there is a value for n and rc.&#x20;

Each n and rc are separate variables so changing it in one function call on stack will have no effect on values held in other factorial function calls on stack.
{% endhint %}

Now, the n is 3 which makes the if statement true. Thus, we make a call to factorial(2).&#x20;

<figure><img src="https://catherine-leung.gitbook.io/~gitbook/image?url=https%3A%2F%2F2335496195-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LFrWzEqLSRHjU6HG9dw%252F-LGxd8qDaQ1QRuKAxM8i%252F-LGxhLpXRO6e1UtvRV5o%252Frecursion4.png%3Falt%3Dmedia%26token%3Dcd26bda8-93fa-49eb-b936-2e843b6d5a57&#x26;width=300&#x26;dpr=4&#x26;quality=100&#x26;sign=22f910b4b1975a8e77a7ab700b39dd4b715e280612cdb436177af2e97c81e651" alt=""><figcaption></figcaption></figure>

Once again, the if statement is true, and thus, we need to call factorial(1).&#x20;

<figure><img src="https://catherine-leung.gitbook.io/~gitbook/image?url=https%3A%2F%2F2335496195-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LFrWzEqLSRHjU6HG9dw%252F-LGxd8qDaQ1QRuKAxM8i%252F-LGxhYnfKBysw0-tqoY_%252Frecursion5.png%3Falt%3Dmedia%26token%3Dcc34e727-a2a0-4f7c-bf16-2c738bd46374&#x26;width=300&#x26;dpr=4&#x26;quality=100&#x26;sign=a7dde341af2711f27a0600df36b6c4ab3090f1f8860a7da6a06015ea40ff60a8" alt=""><figcaption></figcaption></figure>

Once we make this call though, our if statement is false. Thus, we return rc popping the stack&#x20;

<figure><img src="https://catherine-leung.gitbook.io/~gitbook/image?url=https%3A%2F%2F2335496195-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LFrWzEqLSRHjU6HG9dw%252F-LGxd8qDaQ1QRuKAxM8i%252F-LGxhjn50FyG5Hh7khOW%252Frecursion6.png%3Falt%3Dmedia%26token%3D3c431891-24f7-4d60-bcfe-1cfeabc5c9d2&#x26;width=300&#x26;dpr=4&#x26;quality=100&#x26;sign=59651714267254dbc151de0d84117840a35406d0fed4451b274c34f30bc71860" alt=""><figcaption></figcaption></figure>

Once it is returned we can complete our calculation for rc = 2. This is returned, popping the stack&#x20;

<figure><img src="https://catherine-leung.gitbook.io/~gitbook/image?url=https%3A%2F%2F2335496195-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LFrWzEqLSRHjU6HG9dw%252F-LGxd8qDaQ1QRuKAxM8i%252F-LGxiFjxdROzkxRwXlWp%252Frecursion7.png%3Falt%3Dmedia%26token%3D089439db-c937-4fec-99ed-0169f2fcff1f&#x26;width=300&#x26;dpr=4&#x26;quality=100&#x26;sign=8596ec1bf6a307adc00e95a7630bf14d6857dc8f6f57ca84a94c30acfbdbdf7c" alt=""><figcaption></figcaption></figure>

Once it is returned we can complete our calculation for rc = 6. This is returned, popping the stack&#x20;

<figure><img src="https://catherine-leung.gitbook.io/~gitbook/image?url=https%3A%2F%2F2335496195-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LFrWzEqLSRHjU6HG9dw%252F-LGxd8qDaQ1QRuKAxM8i%252F-LGxiMFW_0383mhuZ3IH%252Frecursion8.png%3Falt%3Dmedia%26token%3D7592b777-963e-46a2-ba66-7b81a1c6114b&#x26;width=300&#x26;dpr=4&#x26;quality=100&#x26;sign=457dd630682d42705acfd393167c3dc6415b40803eedfca5033ad06b561fc647" alt=""><figcaption></figcaption></figure>

Once it is returned we can complete our calculation for rc = 24. This is returned, popping the stack&#x20;

<figure><img src="https://catherine-leung.gitbook.io/~gitbook/image?url=https%3A%2F%2F2335496195-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LFrWzEqLSRHjU6HG9dw%252F-LGxd8qDaQ1QRuKAxM8i%252F-LGxiTj639ZIae4mDiLz%252Frecursion9.png%3Falt%3Dmedia%26token%3D42ce6ee4-a60e-45a3-b8a2-5d453e124d8d&#x26;width=300&#x26;dpr=4&#x26;quality=100&#x26;sign=964cf055340484d89b3af603f70637f6e66adbd7de587734b166f30d820d51c9" alt=""><figcaption></figcaption></figure>
