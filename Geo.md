# Geometry Reference

## Basic Definitions

```cpp
typedef ld T;
typedef complex<T> pt;
#define x real()
#define y imag()

// morb3 tool vector or point mn elasl
// lw 3awez eltool 3ltol -> abs(pt p)
T sq(pt p) {
   return p.x * p.x + p.y * p.y;
}

// bt3rfk ent akbr mn 0 wala akl wala ==
// == 1 you > 0
// == -1 you < 0
// == 0 you = 0
int sgn(T val) {
   if (val > EPS) return 1;
   if (val < -EPS) return -1;
   return 0;
}

T dot(pt v, pt w) {
  return v.x * w.x + v.y * w.y;
}

T cross(pt v, pt w) {
  return v.x * w.y - v.y * w.x;
}

// hal v , w 3modyen 3ala b3d?
bool isPerp(pt v, pt w) {
   return fabs(dot(v, w)) < EPS;
}

// vector 3amody 3ala p
pt prep(pt p) {
   return {-p.y, p.x};
}
```

## Transformations

```cpp
pt translate(pt v, pt p) { return p + v; }

pt scale(pt c, ld factor, pt p) {
  return c + (p - c) * factor;
}

// lef p 7walen c b zawya a bel radian
pt rot(pt p, pt c, ld a) {
   pt v = p - c;
   pt rotate = {cos(a), sin(a)};
   return c + rotate * v;
}

// tala3 sora le ay point mn sorten 3ndk
pt linearTransfo(pt p, pt q, pt r, pt fp, pt fq) {
   return fp + (r - p) * (fq - fp) / (q - p);
}
```

## Angles

```cpp
// lw c 3ala shmal ab hyrag3 rkam mogab +
// lw c 3ala el yemen hyrag3 -
T orient(pt a, pt b, pt c) {
   return cross(b - a, c - a);
}

// elzawya ben v,w bel radyan
T angle(pt v, pt w) {
   return acos(clamp(dot(v, w) / abs(v) / abs(w), (T) -1.0, (T) 1.0));
}

// bte7seb elzawya ABC etgah 3ks elsa3a
T orientedAngle(pt a, pt b, pt c) {
   ld ampli = angle(b - a, c - a);
   if (orient(a, b, c) > 0) return ampli;
   else return 2 * M_PI - ampli;
}

T angleTravelled(pt a, pt b, pt c) {
  ld ampli = angle(b - a, c - a);
  if (orient(a, b, c) > 0) return ampli;
  else return -ampli;
}

//check p in between angle(bac) counter clockwise
bool inAngle(pt a, pt b, pt c, pt p) {
  T abp = orient(a, b, p), acp = orient(a, c, p), abc = orient(a, b, c);
  if (abc < 0) swap(abp, acp);
  return (abp >= 0 && acp <= 0) ^ (abc < 0);
}
```

## Lines

```cpp
struct line {
  pt v;
  T c;

  line(pt v, T c) : v(v), c(c) {
  }

  // from equation ax+by = c
  line(T a, T b, T _c) {
     v = {b, -a};
     c = _c;
  }

  //line from two points
  line(pt p, pt q) {
     v = q - p;
     c = cross(v, p);
  }

  // lw p 3ala shmal el line hyrag3 rkam mogab +
  // lw p 3ala el yemen hyrag3 -
  T side(pt p) { return cross(v, p) - c; }
  //bo3d el point 3n el line
  ld dist(pt p) { return abs(side(p)) / abs(v); }

  ld sqDist(pt p) { return side(p) * side(p) / (T) sq(v); }
  //return line 3amody 3lak we bymor be p
  line prepThrought(pt p) { return {p, p + prep(p)}; }
  // hal p zaharet kabl q belnsbh l el line
  bool cmpProj(pt p, pt q) { return dot(v, p) < dot(v, q); }
  // 7arak el line fi etgah vector t
  line translate(pt t) { return {v, c + cross(v, t)}; }
  // return line be3ed 3anak masafa dist
  line shiftLeft(T dist) { return {v, c + dist * abs(v)}; }
  // return point 3ala el line we bt3dy 3ala el 3amody 3aleh
  pt proj(pt p) { return p - prep(v) * side(p) / sq(v); }
  // e3ksha 3ala el line
  pt refl(pt p) { return p - prep(v) * (T) 2.0 * side(p) / sq(v); }

  void print() {
    cout << -v.y <<" "<< v.x <<" "<< -c << endl;
  }
};

// hal l1 and l2 motkat3en -- lw mtkat3en return point el takato3 = out
bool inter(line l1, line l2, pt &out) {
   T d = cross(l1.v, l2.v);
   if (fabs(d) < EPS) return false;
   out = (l2.v * l1.c - l1.v * l2.c) / d; // requires floating-point coordinates
   return true;
}

//return line bynsaf 2 line dakhly or khargy
line bisector(line l1, line l2, bool interior = 1) {
   if (cross(l1.v, l2.v) != 0)return l1; // l1 and l2 cannot be parallel!
   ld sign = interior ? 1 : -1;
   return {
      l2.v / abs(l2.v) + l1.v / abs(l1.v) * sign,
      l2.c / abs(l2.v) + l1.c / abs(l1.v) * sign
   };
}
```

## Segments

```cpp
// hal p gowa circle ab
bool inDisk(pt a, pt b, pt p) {
   return dot(a - p, b - p) <= EPS;
}

// hal p 3ala el segment ab
bool onSegment(pt a, pt b, pt c) {
   return orient(a, b, c) == 0 && inDisk(a, b, c);
}

bool onRay(pt a , pt b , pt c) {
  return orient(a,b,c) == 0 && dot(b-a,c-a) >= -EPS;
}

// hal segment ab yatakat3 m3 cd fi point msh end point
bool properInter(pt a, pt b, pt c, pt d, pt &out) {
   T oa = orient(c, d, a),
        ob = orient(c, d, b),
        oc = orient(a, b, c),
        od = orient(a, b, d);
   // Proper intersection exists iff opposite signs
   if (sgn(oa) * sgn(ob) < 0 && sgn(oc) * sgn(od) < 0) {
      out = (a * ob - b * oa) / (ob - oa);
      return true;
   }
   return false;
}

// return point eltakato3 ben 2 segment
// return 2 or 1 or 0 point
set<pair<ld,ld> > inters(pt a, pt b, pt c, pt d) {
   set<pair<ld,ld> > s;
   pt out;
   if (a == c || a == d) {
      s.insert(make_pair(a.x, a.y));
   }
   if (b == c || b == d) {
      s.insert(make_pair(b.x, b.y));
   }
   if (s.size()) return s;

   if (properInter(a, b, c, d, out)) return {make_pair(out.x, out.y)};
   if (onSegment(c, d, a)) s.insert(make_pair(a.x, a.y));
   if (onSegment(c, d, b)) s.insert(make_pair(b.x, b.y));
   if (onSegment(a, b, c)) s.insert(make_pair(c.x, c.y));
   if (onSegment(a, b, d)) s.insert(make_pair(d.x, d.y));

   return s;
}

// return akrab dist le p 3n el segment ab
ld segPoint(pt a, pt b, pt p) {
   if (a != b) {
      line l(a, b);
      if (l.cmpProj(a, p) && l.cmpProj(p, b)) // if closest to projection
         return l.dist(p); // output distance to line
   }
   return min(abs(p - a), abs(p - b)); // otherwise distance to A or B
}

// return akrab dist le segment ab 3n segment cd
ld segSeg(pt a, pt b, pt c, pt d) {
   pt dummy;
   if (properInter(a, b, c, d, dummy))
      return 0;
   return min({
      segPoint(a, b, c), segPoint(a, b, d),
      segPoint(c, d, a), segPoint(c, d, b)
   });
}
```

## Polygons

```cpp
ld areaTriangle(pt a, pt b, pt c) {
  return abs(cross(b - a, c - a)) / 2.0;
}

// law 3awz t3rf hal el points ely ma3ak 3ks elsa3a area = +
// law 3awz t3rf hal el points ely ma3ak ma3a elsa3a area = -
ld areaPolygon(vector<pt> p) {
   ld area = 0.0;
   for (int i = 0, n = p.size(); i < n; i++) {
     area += cross(p[i], p[(i + 1) % n]); // wrap back to 0 if i == n - 1
   }
   return abs(area) / 2.0;
}

bool above(pt a, pt p) {
  return p.y >= a.y;
}

// check if [PQ] crosses ray from A
bool crossesRay(pt a, pt p, pt q) {
   return (above(a, q) - above(a, p)) * orient(a, p, q) > 0;
}

// strict ya3ny hal el point gowa el polygon tamaman wala la
bool inPolygon(vector<pt> p, pt a, bool strict = true) {
   int numCrossings = 0;
   for (int i = 0, n = p.size(); i < n; i++) {
     if (onSegment(p[i], p[(i + 1) % n], a))
        return !strict;
     numCrossings += crossesRay(a, p[i], p[(i + 1) % n]);
   }
   return numCrossings & 1; // inside if odd number of crossings
}
//return 3adad el points 3ala AB
int laticeAB(pt A , pt B) {
   return gcd(abs(A.x - B.x) , abs(A.y - B.y))+1;
}
//return 3adad el points 3ala adla3 el polygon
// polygon area = I + B/2 - 1;
int calcBoundary(vector<pt>&p) {
   int n = p.size(),ans = 0;
   for (int i = 0 ; i < n ; i++) {
      ans+= laticeAB(p[i] , p[(i+1)%n]);
    }
    return ans - n;
}
```

## Circles

```cpp
//return points el takato3 le line ma3a circle
int circleLine(pt o, double r, line l, pair<pt,pt> &out) {
   double h2 = r*r - l.sqDist(o);
   if (h2 >= 0) { // the line touches the circle
      pt p = l.proj(o); // point P
      pt h = l.v* (T)(sqrt(h2)/abs(l.v)); // vector parallel to l, of length h
      out = {p-h, p+h};
   }
   return 1 + sgn(h2);
}
// return points el takato3 le circle ma3a circle
int circleCircle(pt o1, T r1, pt o2, T r2, pair<pt,pt> &out) {
   pt d=o2-o1; T d2=sq(d);
   if (d2 == 0) {
      //assert(r1 != r2); // may be oo
      return 0;
   } // concentric circles
   T pd = (d2 + r1*r1 - r2*r2)/2; // = |O_1P| * d
   T h2 = r1*r1 - pd*pd/d2; // = hˆ2
   if (h2 >= 0) {
      pt p = o1 + d*pd/d2, h = prep(d)*sqrt(h2/d2);
      out = {p-h, p+h};
   }
   return 1 + sgn(h2);
}

int tangents(pt o1, T r1, pt o2, T r2, bool inner, vector<pair<pt,pt>> &out) {
  if (inner) r2 = -r2;
  pt d = o2-o1;
  T dr = r1-r2, d2 = sq(d), h2 = d2-dr*dr;
    if (d2 == 0 || h2 < 0) {assert(h2 != 0); return 0;}
    for (T sign : {-1,1}) {
       pt v = (d*dr + prep(d)*sqrt(h2)*sign)/d2;
       out.push_back({o1 + v*r1, o2 + v*r2});
    }
    return 1 + (h2 > 0);
}
```
