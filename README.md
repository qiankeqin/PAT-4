### PAT OJ题目
之前也没怎么写过OJ题目，刚接触刷题真的是想死，在这里记录一下自己的代码吧。
如果能帮到也在考研浙大的同学也是一件好事。
* 2018-7-26

  刚开始暑假刷题，好多不会写，心态崩了，握草，看大佬们的代码难的题目要不是解释半天看不懂，要不就是大佬觉得太简单，懒得解释。。。被碾压了。

  接下来打算一个专题一个专题的进行复习。今天主要刷的是树相关的算法，哎！数据结构学的不扎实啊，树的代码没怎么亲自写过，心痛。

* 2018-7-27

  大概写了5、6道左右的树相关算法，主要的就是先序遍历、中序遍历、后续遍历、层序遍历这些变化进行考察，虽然感觉对于我来说还是有点难度。继续加油吧。明天的专题打算写一下 STL的运用以及字符串的处理这两个方面。

  STL 相关的题目一般来说是比较简单的，刷了几道感觉，感觉不错。字符串也是我的一个痛点。看看明天能有什么收获吧。

* 2018-7-28

  果然写到字符串的时候被虐了，好菜啊。记录一下自己学到的处理技巧吧。
  
  * 使用 `cin>>` 进行输入的时候，`\n` 是不会被读入的，所以回车还是在输入缓冲里面。需要使用 `cin.get()` 进行读取
  * `getline(istream, std::string, char delim)` 
  
    一般来说就是 `getline(cin, str)` 这里的只能是c++的标准类 string，而不是 `char` 数组。然后特殊的时候可以利用字符串作为输入，写法如下：
    ```c++
    #include<stringstream>
    string s = "f**k all";
    string container;
    stringstream ss(s);

    getline(ss, container, ' ');
    ```
    这样就达到了分割 string 的效果。
    然后如果有需要使用原始的 char 数组进行输入的话，可以使用 `fgets(char * str, int num, FILE * stream)` 函数。需要指定读入字符的数目，这个数目 `num` 是包括尾部的 `\0` 符号的，所以实际能够输入的数目是 n-1，但是输出 char 数组的时候比较方便，直接 `cout<<str;`

  写了一道 map 使用的题目，感觉自己用不来 STL。。。总结一下题目：
  如果需要使用的一个 key 对应多个不同的 value，而且需要 value 有序。这里注意是不同的 value，并不一定要使用 `multimap`,有时候会显得过于复杂。可以考虑下面的写法：
  ```c++
  #include<map>
  using namespace std;
  map<string, set<int> > mp;
  mp["Tom"] = 12;
  mp["Tom"] = 14;
  mp["Tom"] = 10;
  auto it = mp.find("Tom");// 找到一个 set 结构的指针
  if(it != mp.end()) {
    for(auto i = it->begin();i!=it->end();i++)
      // 进行遍历操作
  }
  ```

  写了好几道 STL的题目，发现自己对于 STL的用法还是不熟悉啊，好多坑等着踩。今天发现一个关于 `set` 的坑。代码如下：
  ```c++
  map<int, int> mp;
  set<int> someSet;// 假设已经有值了
  for (auto it = someSet.begin(); it != someSet.end(); it++) {
		if (mp[*it] == 1)
			someSet.erase(it);
	}
  ```
  删除操作并不会向想的那样，it 迭代器指向的东西已经没了，那 it 应该指向哪里了呢，看了官方文档之后知道了。
  ```c++
  (1)iterator  erase (const_iterator position);
  (2)size_type erase (const value_type& val);
  (3)iterator  erase (const_iterator first, const_iterator last);
  ```
  `erase` 有三种模板，第二种返回的是删除的值，其他两种返回的是删除值下一个地方的迭代器（也就是顺延了一位）。
  如果想要正确执行上面的代码，可以这样写：
  ```c++
  map<int, int> mp;
  set<int> someSet;// 假设已经有值了
  auto it = someSet.begin();
  while(it != someSet.end()) {
    auto now = it++;
    if(mp[*now] == 1)
      someSet.erase(now);
  }
  ```
  因为标准库的设计思想是这样的：
  > The insert members shall not affect the validity of iterators and references to the container, and the erase members shall invalidate only iterators and references to the erased elements.

* 2018-7-29

  今天刷题太难受了，自己太蠢了，好多细节问题的处理都暴露出来了，对于字符串的处理，看了 KMP 感觉有点恐怖，越来越觉得 ACM 大佬太牛逼了，自己还是掌握基本的字符串的处理，这种高阶的算法 PAT怕是不会考，以后有机会再补上吧。

  总结一下字符串处理里面经常用到的函数吧，除了一些脑子瓦特踩的坑，这些常用函数不熟悉踩的坑也很多。

  ```c++
  #include<string> // 基本的函数包含在这里面
  bool isalpha(char a);
  // 判断一个字符是否是英文字母。
  bool isdigit(char a);
  // 判断字符是否是数字

  int stoi (const string&  str, size_t* idx = 0, int base = 10);
  // 一般用不到那么多参数，数字进制一般就是 10，如果想要知道第一个不是数字的位置，可以传进去第二个参数。不过类型一定要用 std:string::size_type *sz，有点麻烦
  std::string str_dec = "2001, A Space Odyssey";
  std::string::size_type sz;   // alias of size_t
  int i_dec = std::stoi (str_dec,&sz);
  std::cout << "firrst non-digit position is  " << sz << '\n';
  // output "firrst non-digit position is 4"

  double stod(const string&  str, size_t* idx = 0);
  float stof(const string&  str, size_t* idx = 0);
  // 这两个用法都差不多，简单用法就是直接送一个 string 进取就行

  string.push_back();
  string.back();
  // 可以很方便的像字符 vector 那样操作

  int tolower ( int c );
  int toupper ( int c );
  // 标准的 c++ string 库好像没有提供直接的大小写转换函数，可以利用下面这个函数进行。需要配合 algorithm 中的 transform 函数进行。输出 "test pat"。转换大写，只要把 tolower() 改成 toupper。
  #include <algorithm> 
  string strA = "test PAT";
  transform(strA.begin(), strA.end(),strA.begin(), ::tolower); 
  ```
  刷题到现在，感觉代码一定要用自己熟悉的函数和库，不然很容易出现 bug，而且一下子找不到，那就真的要哭了。平时多积累常用标准库的用法吧。
  ```c++
  // char 数组转化为 string 对象
  char arr[] = "this is char array";
  string str(arr);// 如果是还没有出现的string对象，可以直接使用构造函数
  str.assign(arr); // 已经存在的string采用赋值函数。

  string substr (size_t pos = 0, size_t len = npos) const; // 一般来说传入两个整型数据就可以了，一个起始下标，一个子串的长度。如果字串长度不写的话，就是后缀
  ```

  明天开始刷排序的题目，试了几道题目，这种题目一般来说不是很难，要不就是卡运行时间，要不就是 `cmp` 函数逻辑比较复杂，一般都可以用结构体解决，当然这里也要涉及基础的字符串的处理过程。

* 2018-7-31

  今天刷了一晚上的排序题目，感觉出的都是很水的题目，有可能是因为大家使用的收拾 c++自带的 sort 函数了，难度都被掩盖了。

  说一个排序题目中经常使用到的技巧吧。很多时候题目会要求给输出排名，但是相同分数的对象要有相同的排名，下一个不同的排名就会有跳跃的情况，为了考场上省时间，还是背一下模板吧。
  ```c++
  int rank = 0, pres = -1;
  // pre 用来记录上一个元素的大小，代码比较简洁，之前很喜欢比较 ans[i] 和 ans[i-1]，但是这样需要考虑边界的情况，太浪费时间了
  vector<int> ans;
  printf("%d\n", (int)ans.size());
  // 输出元素的个数
  for (int i = 0; i < ans.size(); i++) {
    if (pres != ans[i].score) rank = i + 1;
    pres = ans[i].score;
    printf("%d ", rank);
    cout << ans[i].school;
    printf(" %d %d\n", ans[i].score);
  }
  ```

* 2018-8-1

  今天开始看图这一章了，之前对于图可以说是很恐惧的，感觉搜索来搜索去好复杂的样子，而且大部分设计递归的算法，debug真的费劲啊。开始就参考晴神笔记来看，一开始上手肯定是虐的很惨啊。

  参照着晴神和柳神的代码写了两道题，有点收获，还是写一点东西。

  做了一道的利用图的连通量求解的题目，看网上这个可以用并查集和深度搜索来做，就先歇歇深度搜索的算法吧
  ```c++
  vector<bool> visited = {false};
  int G[maxn][maxn] = {0}; // 假设经过输入的赋值，G里面已经存在图的基本信息了。
  int n; // 已经输入了顶点的个数
  void dfs(int node) {
    visited[node] = true;
    for(int i=0;i<n;i++)
      if(G[node][i] > 0 && visited[i] == false)
        dfs(i);
      // 找到 node 的邻接的节点，并且要没有访问过的
  }
  // 主体里面的代码可以这么写
  int count = 0;
  for(int i=1;i<=n;i++) {
    if(visited[i] == false) {
      dfs(i); // 一次深度搜索就会把一个连通分量的所有节点置为已访问了
      count++;
    }
  }
  ```