### 实现Transformer过程中的一些收获

1. 深度学习的模型debug需要注意gpu的并发问题，这会导致debugger无法正常工作，需要将worknum设置为0，相当于取消并发，同时，可以将batch和epoch都设置为1，这样可以加快程序的运行。

2. 看到一个用法感到很神奇：

   ```
   self.to_patch_embedding = nn.Sequential(
       # 这行操作中的h和w很误导人，实际上h和w表示的是每行或每列有多少块patch,h*p1才是原来的h
       Rearrange('b c (h p1) (w p2) -> b (h w) (p1 p2 c)', p1 = patch_height, p2 = patch_width),
       nn.LayerNorm(patch_dim),
       nn.Linear(patch_dim, dim),
       nn.LayerNorm(dim),
   )
   #中间代码省略
   x = self.to_patch_embedding(img)
   ```

   Sequential是一个类，一个类的实例为什么可以给x赋值呢？查资料发现，是因为python中的类通过\_\_call\_\_就可以把类当函数用，至于参数和返回值就看你自己怎么设计了。而Sequential类就是传入一个张量，返回经过各种处理后的张量。