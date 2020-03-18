# Why Functional Programming?

The core of Morphir is the idea of preserving your business concepts (data and logic) in an independent structure.  In order to capture business concepts you first have to be able to express them in a concise and unambiguous way.  What we really want to understand from your application is what its intent is.  In particular, we want to know *what* it should do and *why* without any indication of *how*.  If you know *what* and *why*, it's easier to adjust *how* to different execution contexts.  The flip side of this is that once code starts to specify how it should work, it becomes much more difficult to figure out what the real intent is.  

If you've looked at the topic of programming languages you'll recognize all of this as part of the comparison between various programming styles and functional and imperative languages in particular.  To our great advantage, functional programming has evolved around specifying intent without implementation.  Even better, it's been honing this for decades so it's actually *really* great for modeling business concepts.

Just to give an example, it's difficult to write a programming that figures out the intent of this code:

```java
public Map<String,Object> processSupplies(Supplier[] suppliers) {
  int total = 0;
  int max = -1;
  String maxSupplier = null;

  for(i = 0; i < supplier.length; i++) {
    Supplier supplier = suppliers[i];
    total += supplier.getQuantity();

    if(supplier.getQuantity() > max) {
      max = supplier.getQuantity();
      maxSupplier = supplier.supplierId;
    }
  }
  
  Map<String,Object> result = new HashMap<String,Object>();
  result.put("total", total);
  
  if(max > -1) {
    result.put("maxSupplier", maxSupplier);
  }
  
  return result;
}
```

It's even harder to project that intent into a variety of execution contexts.

On the other hand, this code is easy to interprit:

```elm
processSupplies suppliers =
  let
  total = 
    suppliers
      |> List.map .quantity
      |> List.sum

  max =
    suppliers
      |> List.maximumBy .quantity
      |> List.map .supplierId

  in
    (total, max)
```

That's much easier to process.  It's also much easier to translate into different contexts.  We can turn this into efficient Java code and efficient SQL.  You might say it's inefficient because it's traversing the list twice, once for total and again for max.  The cool thing is that we can recognize this pretty easily and have our code generators optimize the execution to take this into account.

With all of that said, it's certainly possible to derive intent from other types of languages.  In fact, we've found much success in giving a lifeline to the plethora of bespoke expression languages and domain-specific languages (DSLs) that pepper most enterprises.  Parsing these languages into Morphir offers a way to transpile them into other languages and to take advantage of the other tools Morphir offers.  This has been an affective way to upgrade application technologies without risking their important behaviors.