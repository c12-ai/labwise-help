版本号：v1.5.4

# 更新功能：

1. 批量逆合成


# 功能说明

**1、批量逆合成的入口**

![](https://carbon12.feishu.cn/space/api/box/stream/download/asynccode/?code=MGE3YTEzNmJiZGE2ZWJhNWQ3YjFjZGNjNmFhYWFlMDBfZUhXY09ncmtuZWdKSGY5WHdteU80TE9sYnV5Q0hLSVpfVG9rZW46RW1tVWJmUnRibzI3UHp4M3FIVWNLcXB0bktmXzE3NTI0Nzg1NDQ6MTc1MjQ4MjE0NF9WNA)

**2、上传需要批量逆合成的分子list的excel文件，根据示例填写excel**

![](https://carbon12.feishu.cn/space/api/box/stream/download/asynccode/?code=Yjg2OGQ2YWE3OTNkMzg2MzQyM2Q0NWRiNWQzMWI0MGZfOVQ5S3pINUNXQW9YTHIxOVJhQzM0SEJNUWNnNWxoRGFfVG9rZW46UFJWZGJQSXpIb2F2STd4aTc1ZmM2TzZobmJlXzE3NTI0Nzg1NDQ6MTc1MjQ4MjE0NF9WNA)

**3、查看批量逆合成任务的状态，当状态变为“已完成”，则可以点击右侧的“结果下载”，查看批量逆合成的结果**

![](https://carbon12.feishu.cn/space/api/box/stream/download/asynccode/?code=OWQzNjIyMTZhM2JiMzkzMjIzZDBmMjY5NjU4MjBhN2ZfZW5kWHJ0TUNha09mc1AyYW1zSGRFZ3FtczN6OTBHQUhfVG9rZW46SlZNVWJrUDFHbzFuejV4Nm1pWmNyNW41bnJkXzE3NTI0Nzg1NDQ6MTc1MjQ4MjE0NF9WNA)

![](https://carbon12.feishu.cn/space/api/box/stream/download/asynccode/?code=NGQzNmYyNzZjMzI5MWQ3ODZkNDE0NDRkYjIzYmMzY2FfdXdrVXZva1pEVkM0eG9wYXdxZTA4R1l0aUgybFBaa2FfVG9rZW46TVZhemJvZWNibzhXRlR4MjVaMWN2VmhnbmpmXzE3NTI0Nzg1NDQ6MTc1MjQ4MjE0NF9WNA)

**4、结果解读**

输出结果的excel中，指标的含义如下：

**指标含义**

| 字段                    | 样例        | 说明                                                                                                                                                                                                                     |
| ------------------------- | ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| mole\_id                | a1          | 用户输入的mole\_id                                                                                                                                                                                                       |
| smiles                  | C1=CC=CC=C1 | 用户输入的smiles                                                                                                                                                                                                         |
| step\_limit             | 20          | 用户输入的step\_limit                                                                                                                                                                                                    |
| time\_limit\_in\_second | 600         | 用户输入的time\_limit\_in\_second                                                                                                                                                                                        |
| longest\_route\_step    | 10          | 最长的路线长度                                                                                                                                                                                                           |
| shortest\_route\_step   | 4           | 最短的路线长度                                                                                                                                                                                                           |
| median\_route\_step     | 7           | 路线长度中位值                                                                                                                                                                                                           |
| highest\_score          | 0.999       | 路线中算法评分最高的路线分数                                                                                                                                                                                             |
| lowest\_score           | 0.18        | 路线中算法评分最低的路线分数                                                                                                                                                                                             |
| median\_score           | 0.6453      | 路线中算法评分的中位值                                                                                                                                                                                                   |
| status                  | complete   | 1. complete表示已经找到了路线，能找到原料库中的原料合成所需分子1. partial表示有路线，但是没有命中原料库，代表在有限的时间和步数范围内，目标分子能被简化为更简单的分子1. no\_route表示路线在某一层失败，表示很低的可合成性 |
| risk                    | FALSE       | 对complete的情况进一步判断路线是否有风险，详细见下面解读                                                                                                                                                                 |
| step\_score             | 100         | 0\~100的分数，一个暂时针对可合成性的综合评分，详见下面解读                                                                                                                                                               |

**字段解读**

**risk**这个字段，是我们发现有一些有路线（状态complete）的，其中有一些步骤是算法原创的，在文献当中找不到特别相似的反应。这个情况下会标记成risk，总的来说：只有complete状态且排名前五的路线中至少存在一个路线每个反应都找到了较好的文献支持，risk才是False，上面no\_route和partial的，以及complete的路线中没有很好文献支持的，risk = True，您可以认为risk是一个对于是否能合成比较好的bool类型的判断。引入这个变量是为了防止算法本身产生一些“幻觉”，用参考文献的证据来减少false positive。

对​**step\_score**​，我们发现对于一个路线，即使每一个反应都能找到较好的文献支持，但是如果很多只是”较好“级别的反应都出现在一个路线里面，那么这个路线也很难做出来。例如一个30步的路线，每步的成功率都是98%，那么这个路线只有一半概率能做出来。所以这个分数是综合了路线步数和单个反应分数的一个得分。对于步数很长的路线，只有每步反应都有很好的文献支持（基本一样的）才会有很好的得分。目前，我们把这个分数作为可合成性的一个综合分数。

