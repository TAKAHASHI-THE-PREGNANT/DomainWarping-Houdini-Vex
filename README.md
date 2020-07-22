# DomainWarping-Houdini-HeightField
![](https://i.gyazo.com/08b75899b90c51c8df615aea8a24efee.png "")

```
vector fbm(vector pos; float time) {
    vector p = pos;
    vector v = set(0., 0., 0.);
    float a = 0.5;
    float speed = time * 0.003;
    vector shift = set(100 * cos(speed), 100 * sin(speed));
    matrix rot = set(cos(0.5), sin(0.5), -sin(0.5), cos(0.5));
    
    int octave = 7;
    for(int i = 0; i < octave; i++)
    {
        v += fit01(onoise(p), 0.01, 1.8) * a;        
        p = rot * p * 2.0 + shift;
        a *= .5;
    }
    
    return v; 
}

vector domainWarping(vector pos; float time) {
    return fbm(pos + fbm(pos + fbm(pos, time), time), time) ;
}

@density = domainWarping(@P / 1000, @Time) * 100.;
```
