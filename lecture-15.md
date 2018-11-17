## Ranges

```cpp
std::transform
std::copy_it

std::string data = read_file("x.m3u");
std::vector<string> lines;
split(data, '\n', std::back_inserter(lines));
std::transform(lines.begin(), lines.end(), &trim_whitespace, lines.begin());
```
