# 板子与头文件

## ranges
```cpp
#include <utility>
#include <iterator>
#include <algorithm>
#include <numeric>
namespace franges {
  using std::begin;using std::end;using std::cbegin;using std::cend;using std::reverse_iterator;
  template<typename Iter>
  struct base_view {
    Iter l, r;
    typedef Iter iterator;
    base_view(Iter _l, Iter _r) : l(_l), r(_r) {}
    Iter begin() const {return l;}
    Iter end() const {return r;}
    Iter cbegin() const {return l;}
    Iter cend() const {return r;}
    reverse_iterator<Iter> rbegin() const {return reverse_iterator<Iter>(l);}
    reverse_iterator<Iter> rend() const {return reverse_iterator<Iter>(r);}
  };
  namespace vw {
    template<typename Container>
    base_view<typename Container::iterator> view(Container& con) { return base_view<decltype(begin(con))>(begin(con), end(con)); }
    template<typename Container>
    base_view<typename Container::iterator> view(const Container& con) { return base_view<decltype(cbegin(con))>(cbegin(con), cend(con)); }
    struct cut {
      int l, r;
      cut(int _l, int _r) : l(_l), r(_r) {}
      cut(int _r) : l(0), r(_r) {}
      template<typename Range>
      decltype(auto) operator() (Range& range) const {
        auto itl = begin(range), itr = begin(range);
        std::advance(itl, l), std::advance(itr, r);
        return base_view<decltype(itl)>(itl, itr);
      }
      template<typename Range>
      decltype(auto) operator() (const Range& range) const {
        auto itl = cbegin(range), itr = cbegin(range);
        std::advance(itl, l), std::advance(itr, r);
        return base_view<decltype(itl)>(itl, itr);
      }
    };
    struct drop {
      int x;
      drop(int _x) : x(_x) {}
      template<typename Range>
      decltype(auto) operator() (Range& range) const {
        auto itl = begin(range);
        std::advance(itl, x);
        return base_view<decltype(itl)>(itl, end(range));
      }
      template<typename Range>
      decltype(auto) operator() (const Range& range) const {
        auto itl = cbegin(range);
        std::advance(itl, x);
        return base_view<decltype(itl)>(itl, cend(range));
      }
    };
    template<typename Value>
    class value_iterator : public std::iterator<std::random_access_iterator_tag, Value> {
      Value x;
      using this_iterator = value_iterator<Value>;
      public:
      value_iterator() = default;
      value_iterator(int _x) : x(_x) {}
      value_iterator(const this_iterator&) = default;
      value_iterator(this_iterator&&) = default;
      friend bool operator==(const this_iterator&lhs, const this_iterator& rhs) {return lhs.x == rhs.x;}
      friend bool operator!=(const this_iterator&lhs, const this_iterator& rhs) {return lhs.x != rhs.x;}
      friend bool operator<(const this_iterator&lhs, const this_iterator& rhs) {return lhs.x < rhs.x;}
      int operator-(const this_iterator& oth) const {return x - oth.x;}
      this_iterator operator+(int oth) const {return x + oth;}
      this_iterator operator-(int oth) const {return x - oth;}
      this_iterator& operator+=(int oth) {x += oth; return *this;}
      this_iterator& operator-=(int oth) {x -= oth; return *this;}
      this_iterator& operator++() {++x;return *this;}
      this_iterator& operator--() {--x;return *this;}
      int& operator*() {return x;}
    };
    using num_iterator = value_iterator<int>;
    base_view<num_iterator> range(int l, int r) { return base_view<num_iterator>(l, r); }
    struct reverse_t {
      template<typename Range>
      auto operator()(const Range& v) const {return base_view<decltype(rbegin(v))>(rbegin(v), rend(v));}
      template<typename Range>
      auto operator()(Range& v) const {return base_view<decltype(rbegin(v))>(rbegin(v), rend(v));}
    } reverse;
  }
  namespace fn {
#   define movefunc(funcname) const auto funcname = [](auto& cont, auto... args) {return std::funcname(begin(cont), end(cont), args...);}
    movefunc(sort); movefunc(reverse); movefunc(fill); movefunc(iota); movefunc(partial_sum);
#   undef movefunc
  }
}
template<typename Value, typename Func>
auto operator|(Value&& l, Func r) { return r(std::forward<Value>(l)); }
template<typename Value, typename Func>
Value& operator|=(Value& l, Func r) { return l = r(l); }
#define molambda(expr) ([&](const auto& _) {return expr;})
#define bilambda(expr) ([&](const auto& a, const auto& b) {return expr;})
```

## covsys
```cpp
#include <numeric>
namespace fmath {
  using std::int64_t;
  template<typename T, T mods>
  struct modint_traits {
    using promote_t = int64_t;
    static T reduce(T a) {(a < mods ? a -= mods : void()), (a > mods ? a += mods : void());}
    static promote_t promote(T a) {return static_cast<int64_t>(a);}
    static T mod(promote_t a) {return a % mods;}
  };
  template<typename T, T mods, typename modtraits = modint_traits<T, mods>>
  struct basic_covsys {
    static T multiple(T a, T b) {return modtraits::mod(modtraits::promote(a) * b);}
    static T add(T a, T b) {return modtraits::reduce(a + b);}
    static T sub(T a, T b) {return modtraits::reduce(a - b);}
    static T power(T a, T p) {
      T r = 1;
      while (p) {
        if (p & 1)
          r = multiple(r, a);
        a = multiple(a, a);
        p >>= 1;
      }
      return r;
    }
    static T inv(T a) {return power(a, mods - 2);}
    static T factorial(int x) {
      T r = 1;
      for (int i = 2; i <= x; ++i)
        r = multiple(r, i);
      return r;
    }
    template<typename Beginer>
    static void make_factorial(Beginer l, int n) {
      *(l++) = 1;
      T la = 1;
      for (int i = 1; i < n; ++i)
        ++l, la = *l = multiple(la, l);
    }
    template<typename Beginer>
    static void make_invfactorial(Beginer l, int n) {
      T la = inv(factorial(n - 1));
      l += n - 1;
      for (int i = n - 1; i; --i, --l)
        la = *l = multiple(la, i);
    }
  };
  template<int mods> using int_covsys = basic_covsys<int, mods>;
  using covsyspoly = int_covsys<998244353>;
  using covsys1e9_7 = int_covsys<1000000007>;
}
```