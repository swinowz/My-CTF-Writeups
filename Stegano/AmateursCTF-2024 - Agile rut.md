#WEB #Steganography 
Se rendre sur le site web, dans source télécharger la police ( .otf ) 

utiliser le script python ( dispo sur mon github ) : OTFDumper.py

Le flag dans la sortie donnée par le script : 
```python
<LigatureSet glyph="a"> 
components="m,a,t,e,u,r,s,c,t,f,braceleft,zero,k,underscore,b,u,t,underscore,one,underscore,d,o,n,t,underscore,l,i,k,e,underscore,t,h,e,underscore,j,b,m,o,n,zero,underscore,equal,equal,equal,braceright" 

glyph="lig.j.u.s.t.a.n.a.m.e.o.k.xxxxxxxxx.xxxx.x.xxxxxxxxxx.x.x.x.xxxxxxxxxx.xxx.xxxxxxxxxx.x.x.x.x.xxxxxxxxxx.x.x.x.x.xxxxxxxxxx.x.x.x.xxxxxxxxxx.x.x.x.x.x.xxxx.xxxxxxxxxx.xxxxx.xxxxx.xxxxx.xxxxxxxxxx"/>
```
En faisant un strings simple sur la police, on aurait juste eu le faux flag ( dans la variable glyph )