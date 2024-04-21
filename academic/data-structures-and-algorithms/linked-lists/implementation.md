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

# Implementation

## <mark style="color:red;">push\_front(data)</mark> <a href="#push_front-data" id="push_front-data"></a>

The push\_front function adds a node to the front of the linked list.&#x20;

#### As with all insertion of nodes, the process can be generally described in the following manner:

1. Create a new node with the appropriate data
2. Link the new node up with the rest of the list

Let us start by considering how this would work in a general linked list.&#x20;

#### The steps are:

Step 1: Create new node, next node is the current front of list, there is no previous node which should be set to a nullptr

<figure><img src="../../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Step 2: Make the previous node of the current front of list point to the new node

<figure><img src="https://catherine-leung.gitbook.io/~gitbook/image?url=https%3A%2F%2F3939692842-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LFrWzEqLSRHjU6HG9dw%252F-LLFdsDKIotqGCLoyHkm%252F-LLGlOuUxa_OrFwV2gs7%252Fpushfront2.png%3Falt%3Dmedia%26token%3Dc221721e-7880-4c2e-b306-1f8e29ee7d50&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=cb50216f9ddb3c631a42758a982d8f5c2d47b97a2f67a90c37eebf412c18a700" alt=""><figcaption></figcaption></figure>

Step 3: Make the front pointer point to the new node

<figure><img src="../../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

#### Putting this together in code:

```cpp
Node* nn = new Node(data, nullptr, front_);
front_->prev_=nn;
front_=nn;
```

### Does above always work? <a href="#does-above-always-work" id="does-above-always-work"></a>

Now lets consider if the following would work if we started off with a linked list that is empty.

Step 1:

<figure><img src="https://catherine-leung.gitbook.io/~gitbook/image?url=https%3A%2F%2F3939692842-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LFrWzEqLSRHjU6HG9dw%252F-LLFdsDKIotqGCLoyHkm%252F-LLGlGwSvvGgo9EhGekj%252Fpushfront4.png%3Falt%3Dmedia%26token%3D87d89f7c-fa8e-4e48-95c6-0740c49bb13d&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=3168284d5e844b3c307ed862b79b3d533da0ae1a52a636f09d4ab2bf04f8a867" alt=""><figcaption></figcaption></figure>

Step one will run without problems

Step 2:

<figure><img src="https://catherine-leung.gitbook.io/~gitbook/image?url=https%3A%2F%2F3939692842-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LFrWzEqLSRHjU6HG9dw%252F-LLFdsDKIotqGCLoyHkm%252F-LLGlt6pHyhUxQJBGVtr%252Fpushfront5.png%3Falt%3Dmedia%26token%3Dff920c27-532c-45dd-9214-edf72e7e2566&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=f150a8cddafce115ad3f5c3c4f94ba9a93577b266903efd276382b6a734402f3" alt=""><figcaption></figcaption></figure>

front\_ is nullptr, so front\_->prev\_ will crash. Thus, it looks like we need to skip this step or do something different if we have an empty list

Step 3:

<figure><img src="https://catherine-leung.gitbook.io/~gitbook/image?url=https%3A%2F%2F3939692842-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LFrWzEqLSRHjU6HG9dw%252F-LLFdsDKIotqGCLoyHkm%252F-LLGn1MHsNee-3k17q0y%252Fpushfront6.png%3Falt%3Dmedia%26token%3Dd2cebfa1-2efe-41d2-9f22-436c323785de&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=bb680580a2a8896bd5ddce562291005e08141c992c5adbb2fab507d765dc886a" alt=""><figcaption></figcaption></figure>

The above will work. However, if we were to simply skip step 2, our linked list would not be valid as back\_ would not be correctly set.&#x20;

The node we just added is not only the first node in the linked list but also the last.&#x20;

{% hint style="info" %}
To make this work, we should add code to make back\_ point to nn.
{% endhint %}

#### Putting all this together, The final function would look like the following:

Copy

```cpp
void push_front(const T& data){
    Node* nn = new Node(data, nullptr, front_);
    if(front_){
        front_->prev_=nn;
    }
    else{
        back_=nn;
    }
    front_=nn;
}
```

## <mark style="color:red;">pop\_front()</mark> <a href="#pop_front" id="pop_front"></a>

The pop\_front() function removes the first node from the linked list.&#x20;

#### The following are the general steps to remove a node:

1. Check to make sure that the list isn't empty (or that the node to be removed actually exist).
2. Unlink the node to be remove from the list (ensure other nodes are not lost in the process)
3. Deallocate the memory for the node

#### Given the above steps, let us consider how this would work in the general case. The steps for performing a pop\_front are:

Step 1: Check to make sure list isn't empty. If it is do nothing. Otherwise continue to next steps

Step 2: Make a local pointer point to the first node in the list (hold this node so we don't lose it by accident)

<figure><img src="https://catherine-leung.gitbook.io/~gitbook/image?url=https%3A%2F%2F3939692842-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LFrWzEqLSRHjU6HG9dw%252F-LLFdsDKIotqGCLoyHkm%252F-LLGnWbnJA9YcwaKdYjA%252Fpopfront1.png%3Falt%3Dmedia%26token%3D5f5c854d-a82b-4bee-a3b2-67d5882f9eaa&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=5b5b7e823c9260c894bdac941c574525323d3ab26cf6d4d1c8bd80e1bfa941a4" alt=""><figcaption></figcaption></figure>

Step 3: Make the front pointer point to the second node

<figure><img src="https://catherine-leung.gitbook.io/~gitbook/image?url=https%3A%2F%2F3939692842-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LFrWzEqLSRHjU6HG9dw%252F-LLFdsDKIotqGCLoyHkm%252F-LLGna-t-VkMTZKBMzLx%252Fpopfront2.png%3Falt%3Dmedia%26token%3De16618f7-3d57-45c4-99c6-075190597e91&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=0b5d5ddcca85c56ca20a16288827db26ddeb50b97e796730a8089fe6909054c1" alt=""><figcaption></figcaption></figure>

Step 4: Make the new front node's previous pointer a nullptr

<figure><img src="https://catherine-leung.gitbook.io/~gitbook/image?url=https%3A%2F%2F3939692842-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LFrWzEqLSRHjU6HG9dw%252F-LLFdsDKIotqGCLoyHkm%252F-LLGnepgcUkdws2Ueguh%252Fpopfront3.png%3Falt%3Dmedia%26token%3D41a0015f-384d-4fa7-a383-e3ed15ffff20&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=df7dd2a8cf777d0eaa4d90170d1e399734f430319d165f452c04950c1167b000" alt=""><figcaption></figcaption></figure>

Step 5: deallocate the node we are removing

<figure><img src="https://catherine-leung.gitbook.io/~gitbook/image?url=https%3A%2F%2F3939692842-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LFrWzEqLSRHjU6HG9dw%252F-LLFdsDKIotqGCLoyHkm%252F-LLGnkBz4HLhKupdWtGG%252Fpopfront4.png%3Falt%3Dmedia%26token%3D7a00f7ef-cc16-4838-a202-8861bc8d7b7d&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=809987b2bb93438a6c5453cefd8951f501381dd8c77390fbf9796f250b6d8fcd" alt=""><figcaption></figcaption></figure>

**`delete p;`** does not deallocate the pointer _**p**_ itself. Rather it deallocates what the pointer points at. Thus, **`delete rm;`** deallocates the node rm points at.

#### The code snippet to do this is:

```cpp
if(front_){
    Node* rm = front_;
    front_=front_->next_;
    front_->prev_=nullptr;
    delete rm;
}
```

A common misconception is that when you declare a pointer, you must use new. Unless you are trying to create an instance of the object, you do not need to use new.&#x20;

If you are using the pointer to simply refer to something that exist (like rm in the code above), there is no reason to use new and in fact, if you used new, you would end up with a memory leak.

### Does the above always work? <a href="#does-the-above-always-work" id="does-the-above-always-work"></a>

The snippet of code above works for the general linked list and empty linked lists. However, does it always work? Let us consider the following situation. If we only had one node left in the list, would the above snippet still work?

#### If we perform the four steps inside the if statement from the above snippet, this is what we will see:

Step 2:

<figure><img src="https://catherine-leung.gitbook.io/~gitbook/image?url=https%3A%2F%2F3939692842-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LFrWzEqLSRHjU6HG9dw%252F-LLFdsDKIotqGCLoyHkm%252F-LLGoCITmpS-b0YLx4IT%252Fpopfront5.png%3Falt%3Dmedia%26token%3D14b18054-9b7a-4306-ac0a-3dc571ec1b35&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=9fdcd5b039b77ddcd3eebdb5677641bfc6f66653285e54ebcec98af52a60fde7" alt=""><figcaption></figcaption></figure>

Above looks fine.

Step 3:

<figure><img src="https://catherine-leung.gitbook.io/~gitbook/image?url=https%3A%2F%2F3939692842-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LFrWzEqLSRHjU6HG9dw%252F-LLFdsDKIotqGCLoyHkm%252F-LLGpSrPHL643u_5GDXo%252Fpopfront6.png%3Falt%3Dmedia%26token%3D806969b8-808e-4305-a386-fdf19b8d6a13&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=e2ae024a50ae282278e40347f04a815ab3e4edbca047668c847e030ec1a3422a" alt=""><figcaption></figcaption></figure>

This also looks correct

Step 4:

<figure><img src="https://catherine-leung.gitbook.io/~gitbook/image?url=https%3A%2F%2F3939692842-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LFrWzEqLSRHjU6HG9dw%252F-LLFdsDKIotqGCLoyHkm%252F-LLGpaX3ucD-Bh8N96Qj%252Fpopfront7.png%3Falt%3Dmedia%26token%3D621cefb9-0efb-466d-ad11-31a726b3ffd4&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=c989712b56adb5072eafd6dd893225589c1fcdb0fa630b4c4a4ada9856dfc112" alt=""><figcaption></figcaption></figure>

If we were to try to do the above at this point we would end up crashing the program as front\_ is currently nullptr. This step will either need to be skipped for lists with just one node or something differnt will need to be done

Step 5:

<figure><img src="https://catherine-leung.gitbook.io/~gitbook/image?url=https%3A%2F%2F3939692842-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LFrWzEqLSRHjU6HG9dw%252F-LLFdsDKIotqGCLoyHkm%252F-LLGppMskr6k95FibkNp%252Fpopfront8.png%3Falt%3Dmedia%26token%3D27be65d2-8655-4850-8937-d7fffa78e11b&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=0c7e3dee1a3f0468a14022d89e4a3b32209d814617d39ec57d1bc73f589de475" alt=""><figcaption></figcaption></figure>

The final line is to delete the node, which won't crash. However, the list is not in a valid state. We have a back\_ pointer that points to the memory location of the node that has now been deallocated.&#x20;

This pointer is therefore invalid. For lists with just one node, we must also adjust the back\_ pointer to a nullptr as the list is now empty

#### Putting all this together our pop\_front() function should look something like this:

```cpp
void pop_front(){
    if(front_){
        Node* rm = front_;
        front_=front_->next_;
        
        if(front_==nullptr){  //if only one node exists
            back_=nullptr;
        }
        else{
            front_->prev_=nullptr;
        }
        delete rm;
    }
}
```
