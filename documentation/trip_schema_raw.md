---
layout: page
title: Trip Schema - Raw
permalink: /trip-schema/raw
---

This schema matches exactly what comes from the device post de-compression. 

Data is compressed on the device pre-transmission using <a href="https://msgpack.org/">MessagePack</a> before being unpacked server-side and parsed to the object type below.

Trip data is collected every 1 second, with data transmission of normal driving data 10 minutes post trip completion. 

```go
type Data struct {
    DeviceID  string
    TripStart int
    Points    []Point
 }
  
 //Point is raw data collected by a little theo device
 type Point struct {
    Timestamp         int
    BatteryVoltage    int
    BatteryPercentage int
    Ax                float64
    Ay                float64
    Az                float64
    MinAx             float64
    MinAy             float64
    MinAz             float64
    MaxAx             float64
    MaxAy             float64
    MaxAz             float64
    Lng               float64
    Lat               float64
    Heading           float64 
    Speed             float64  // Knots
    HDOP              float64 
    Quality           int     
    Satellites        int     
 }
```

<h2>Sample data</h2>

<h3>Raw JSON output</h3>

```
{% for point in site.data.samplerawtrip %}
{{ point }}
{% endfor %}
```

<h3>Polyline</h3>

Inspect the below polyline here: <a href="https://developers.google.com/maps/documentation/utilities/polylineutility">https://developers.google.com/maps/documentation/utilities/polylineutility</a>

```
c`ypIzzyH@`@?@?AB@@??@CCEEGE??ECCA@@A????AA?EEAA@?@?@????????A???A?AAACC??C?????A???A??????@??A???????????@@?@@???????A??A??A??A???????A????????????A???????????????????A?????ACCE??A???????@@???????A??AE@???@@@C@CAAA??A???????A?????????AA??A?????????????@????A??C?A??@??????@?@????A??????????????@?????@?@?@???????A?????????A?C?A?A????I?E@GBE@E@E@CBI@GBG@G@EBIBIDGDMH??IFGDIFGDGBE?GECIAM@IBGHEHGJEFCLE??FCJIFCHEDGBC@C?AD?D?L@J@P@L?P@T???PBN?T@R@L@V@RBL@V@P@J?R@L@H?HCDK??@QASAY?YD[DUL_@LSPKRGNEXGVCNCXITI??RITKNGRGTKLGVKPGLEXMPKRIPIJGPMDQ??BQAWAUEc@Ec@E_@Go@Ei@Aa@Aw@Bg@B{@Dm@Hg@Nw@Lm@??Je@Ru@Lm@Hc@Ps@Lg@H]Lm@F]Li@Hc@H]Jk@Ja@FYJi@??Ha@F]Jo@H_@Hi@Lq@Hi@Jk@Hi@Fa@Lq@Hi@Ha@Ls@Jk@Ha@??Pu@Lk@Ja@Ps@Jc@Rs@Ni@Lc@Tu@Pm@Na@Vs@P_@Ri@Xs@P_@??Xm@Te@N_@Zk@P]Tc@Xk@Rc@P[Vi@Tc@N[Xi@N[Xi@Ra@??N[Xi@Ra@P]Vg@Va@P[\g@RY\c@X[RU\[VUROZU??TOPKZQTKPIXKRITGTGNEVGRENCXERANA??XAP?N?T?P?RBP@N@TBPDNBTBN@N?TANC??PGVONMLWRYLQRYNSRWLQNSNWJMTc@FM??FW?W?SE[EQG[GSCOAYBSFOJWLQLSLQJK??RQLKPMRMPKNETILERGTKRINGVKTKTKTI??NIZKVKPI\MVKRG\ER?\AXCRE`@IZMTK^U??TO`@YZUTO`@WXURUX_@PYVc@Pa@JYPg@L]HWL_@??HWDODYBMBW?UAOIUGKECIAECEGCO@ODM??FOHODMFU@MAUGOGKMSIMGIKIKAGBGHAH??ENGTCNGVCNALCL?H?FDFBBDBF@F?F@H???F?FEFKBIBG@C?????????????????????????????????????????????????????????????????????????????????????????????????????????????A??????????????????????A????????????????????????????????@?????????????????????????????@??????????????????????A??????????????????????????????????????????????????????????@?????A?????@??????????????????????????????????????A?????????????????????????????????????????FB?????????????????????@??????????????????????????????AA@@???????????????????????????????@???????????????????????????????????????????????????????????@???????????????A????????????????????????????????????????????????????AEAGCEACEE??GGGEEIGICECGCEAO?KDQFKHIL?LFVV??JJJJFFDDBJ?PALETCPCNCT@RBJJLJFB@??D@FA@EHQHOJWLa@JUPe@Ra@NWTa@VYPQXSTO??RM\SRIVM^MXKRIZMPIVQNQJOHWJYHKPG??LBLFRJXFND\EZKVG`@OXK^Of@QXK^Of@QXK??d@Q`@MXMf@Q^MXKd@O^O\K^OXId@Q\K\G\EXC??b@E^AV?d@BX@b@D^DVBd@J\HVFd@J\H^H^JXF??h@L`@HVHf@JXFf@H^FX@d@AX?f@?`@BZDh@Jb@N\L??n@P^Jl@Nf@J`@Dn@Jf@H`@Hn@J^Fp@Jf@F^Fl@H^Bd@B??l@B\@j@Ad@A\Cl@Gd@E^En@Id@G^En@I^Ef@Gn@I\C??p@E^Af@@`@@n@Fh@Hf@Lh@P^Nn@\f@\d@^d@b@j@n@b@j@Xb@??`@n@`@r@`@r@h@~@^r@`@r@`@r@`@t@`@r@`@r@Zf@h@x@`@n@\b@h@r@d@j@??Zb@l@r@\b@j@r@b@j@Zb@j@r@\b@j@r@b@h@b@j@b@j@Zb@j@t@\b@d@j@??l@t@^b@d@l@n@x@\b@l@v@d@j@\b@h@r@`@f@Z\d@j@XXf@f@^\ZV??f@`@XTf@^`@VXPf@Zb@X`@Zb@Z\Vl@b@d@^^XtAdA`@Z??p@d@h@^`@Xp@f@`@Xr@f@h@^b@Zr@d@`@Xh@`@t@`@b@Vt@^l@Vb@P??v@Vn@Rb@Jx@Pd@HdBTd@Bv@Db@Bl@Br@D`@Bp@Fh@F^F??n@L\Jn@Pf@P^Np@Vf@T`@Rp@\h@Z`@Tr@`@j@X`@Tp@\`@N??p@Vh@N`@Jt@Nj@Jb@Ft@Fb@Dv@Fl@Bd@Dv@Fd@Bv@Fl@Dd@B??t@Fn@Db@Dx@Dd@Bx@Fn@Dd@Dx@Fp@Ff@Dz@Ff@Dn@Hz@Ld@H??x@Pp@Nd@Lv@Tn@Rb@Pv@Zl@Vb@Rt@`@j@Zb@Vt@d@b@Xj@^r@h@??`@Zp@j@f@b@^Zn@h@f@`@^Zp@d@`@Xh@Zr@`@`@Rj@Tp@Z`@Nr@T??j@P`@Lt@Nj@Lj@Hb@Fl@Fr@Dl@D`@@r@@b@Aj@Ar@Cb@Aj@G??t@I`@Gj@Mr@O^Ir@Qh@O`@Mr@Wh@U^Sp@]f@Y^Sn@_@\U??b@]l@c@\WrAcA\Wb@]j@a@ZUh@a@`@[XSb@[ZUTQ`@U??ZORMXMNGRKROLKNSTSLGTAPLPNNJXJPB??^BT@b@BV@`@@`@Bh@B`@B\@j@BZ?b@Bh@BZBd@@j@D??ZDb@@n@B\Bn@Bf@@\Bn@Bf@B`@Bp@D^@f@Bn@D\Dl@H??b@JXHb@PZJPHXFR@J?RELGNSRYNUVa@\i@T]??Zi@b@q@Vc@d@s@^k@Xc@d@w@`@o@Ze@f@y@`@o@Zg@f@y@^m@Xe@f@u@??^k@Xc@b@w@Xc@^k@b@q@Va@\i@d@o@Xa@d@o@b@e@\_@h@k@d@e@^[??l@i@f@a@^[n@e@f@[^Up@_@`@Uh@Up@[`@Oh@Sr@Ub@Kh@Mr@O??b@Ih@It@I`@Ct@Gj@A`@At@Ah@?`@Ar@?j@?`@?r@Ab@@j@A??p@Ab@Ch@Cp@G^Gp@Oh@O^Mp@Uf@W^Un@a@^Wl@g@^[j@k@??\]d@e@b@e@h@k@\[b@c@h@k@b@e@Z[b@e@j@o@\_@h@q@b@k@Za@f@w@??b@m@Xg@f@y@`@q@Xi@b@{@Vi@\s@b@_AVi@Zw@`@eATo@Z{@\kATq@??X_A\oARs@\qAVeARw@\uAVgAPw@\sATcAPu@ZqAVcAPw@ZoA??TcAPw@ZoAPu@TaAXmAPs@XkAT}@Pq@XgAXw@Ri@\{@Zm@T_@??d@m@V[d@c@^U^QVI`@MTEZG^GTAZCZ@N@LDJ@??LDH@H?HANANDJHNVNTLPVPVFV@b@EXE`@M??b@Md@Ol@S\Kn@S^Mn@S`@OxAe@`@Mr@Wj@Q`@Mr@U`@O??h@Qr@U`@On@Uh@Q^Mn@Sh@M^Kn@Mh@I`@Ep@Gf@A`@@n@@??f@B^Bn@F^Fd@Jn@N^Jp@Rd@N^Nn@V^Nl@X^Nf@Rl@T??d@T^Ld@Rl@V\Lj@Td@P\Lf@R^NVH^NRFVFVBN@??TANKFQHc@He@Dc@Jy@Hk@N{@PeALo@Ty@ZgATo@Zy@`@cA??Vm@^u@d@aA`@w@Vk@f@_AXk@^w@d@_AXk@f@_A^u@Xi@d@aA^u@Xi@??d@aAXi@^w@f@aAXk@f@aA^u@Zm@f@cA`@w@Zo@f@cA\m@`@y@h@gA\m@??j@cAd@u@\i@n@{@\e@f@m@p@q@\_@f@g@l@o@\_@d@g@j@o@Za@b@k@f@w@??Xe@^o@d@}@Ti@x@kBZw@\cATk@Vu@\_ARk@Vu@\_APi@Tq@??Zy@Pe@Tq@X{@Ni@Rw@TeALo@d@iCJs@RkAP_ALq@N}@RgA??Lq@TiALo@TgALq@P}@P_ATkANq@N_ATkANq@R_ARkALq@P}@??TkALo@TiAP{@Nm@XcATw@Pi@\}@Rg@Zk@b@s@Va@b@m@^a@VW??b@_@VSZS^YZSROX_@NWN]Nc@HUDYDQ@I@O@K??@I@SDOFGL@HJLRPXLNTT\XRPXX`@\TRZT??`@ZRPZR\VRNZTVNPJVHL@L@LAHEBEDE?C??@?????????@CDGFSHQN[NSNKXIV@PHXT??LTRXXRRF^A\GVGf@Kb@MZKn@S`@Oh@Ut@_@`@Ul@]??r@g@`@]t@m@j@g@`@a@t@u@j@m@^g@r@}@`@g@h@s@r@}@`@i@h@s@t@}@`@i@??r@_Aj@u@`@i@t@{@l@m@b@c@x@s@d@a@n@i@z@o@d@[|@i@n@]h@Sz@]p@W??f@M|@Uh@Kp@K|@Kh@Ep@Cz@Ad@@x@Bl@Fd@Br@Jj@H^Hn@N??b@JZJf@PXJ^N^PNFRJPDL@PCNKLKTYPONE??XAPDVPT`@LZNf@Tt@Ld@n@vARVb@ZZPh@^\Td@Z??h@\t@f@d@Xp@`@p@d@p@b@|@l@h@^|@l@t@d@t@f@h@^t@d@`An@h@^t@d@??`Al@h@\`Aj@h@Z`Al@f@\`Al@h@\r@f@~@l@h@\t@d@r@d@~@l@r@d@h@^??r@d@|@n@h@^r@j@~@n@j@`@r@f@`Ap@j@^t@d@`An@j@^t@d@`Al@j@\bAj@??t@d@l@^~@l@t@f@h@\`Al@h@^t@f@~@r@h@`@r@j@|@r@h@b@p@l@|@v@d@d@??p@l@n@n@n@l@p@n@n@n@n@l@x@x@f@d@n@n@n@n@p@p@p@l@n@p@p@n@p@p@p@n@??x@z@f@f@p@n@z@z@f@f@x@|@n@r@f@f@z@z@d@d@lBjBf@d@x@v@p@l@f@b@??z@v@d@b@n@l@v@v@d@d@n@l@v@t@d@b@v@v@l@l@b@b@v@t@d@f@n@l@v@t@b@b@??v@v@n@l@d@d@v@r@d@`@p@f@z@j@h@Xp@^z@^h@Rz@Vf@Jp@Lx@Jh@D??p@D|@Bj@?~@Ef@Gt@K|@Sh@Mr@Qz@Uf@Mr@Oz@Qf@Kx@Qp@M??f@Kl@Mn@Mf@Ix@Mn@Mp@Mp@Mn@Op@Mp@Op@Oz@Oh@Mz@Q\G??p@Oz@Qr@Qt@Of@Q|@Yz@[f@Up@[p@]n@_@p@a@p@_@x@g@p@c@p@a@??f@[p@a@r@]t@]t@Y`AYt@QfDg@x@Cl@@bA@j@DlBT??t@N`AVh@P~@^r@\f@Xz@h@f@^x@n@l@j@d@`@t@x@`@f@r@~@f@v@^j@??n@fA\n@d@|@j@hA\n@h@hAb@z@Zl@h@dA\l@b@t@l@~@\j@n@|@f@n@`@d@??r@r@b@`@l@f@v@j@d@Vp@\z@\h@Tr@V~@Xh@P|@Vr@Th@Pz@Pj@R??t@V|@\h@Rt@V~@Xh@P~@Xt@Th@Nt@R~@Vh@N~@Rr@Rj@N~@R??j@J~@Rr@Nj@J~@Pj@Fr@H~@Fh@Dt@B~@Bj@?r@?`A?h@?`A@??t@@h@@`A@t@Bh@@`A@t@@j@@bABj@@t@@v@@t@@v@D`ADj@B??v@BbABj@@v@@bA@j@@bABl@@v@@v@@v@@x@@z@@z@?x@?|@???z@?fA@z@Bn@@z@BhA@n@@fABn@@x@@z@@z@@dABz@@x@@l@@??dABl@@x@@bA@l@@v@BbABj@@v@@bA@l@B`ADv@Dj@B`AHt@F??l@F`ALj@Hv@N`ARj@Lv@R`AXj@P`A^t@Xl@T~@`@j@Vt@`@`Ah@??h@\~@l@r@d@h@^~@j@r@f@h@\~@l@h@^r@d@~@j@h@\r@d@|@l@f@\r@b@??z@j@f@Zz@l@f@\n@d@n@d@p@d@z@j@h@Zn@b@|@h@f@Zp@b@z@j@d@\p@b@??z@h@f@Zn@b@v@h@d@Xj@^p@`@^Tl@`@b@XZR`@V^TZT^TPN??VNXNNHPJPJFDFBD@@@@????????????????????????@????????????????F@HBJD??PJJFPJLHf@RNBF@JALEFGLKJQFIJM??FGDEDC@A????????????????????DADA??J@LDHFLLHLBHDJBD@B@B???@??????BH??BJDX?XAREb@I\ITSZORSVU^KXIf@Kt@Gh@KbA??Gp@I`AMlAGt@KdAMtAIhAI|@K`BI`AKtAKhBIdAIxAKjBGdA??GtAIfBEbAGfBErAEbAEdBCbAEtACfBCbACrACfBAdAAtACvA???vAAhB?dA?xA?lB?fA?|A?nB?jA?|A?pB?jA?|A?pB?jA?~A??ApB?hA@|A?nB?jA?|A?nB?jAApB?|A?jAArB?hA?~A?nB?hA???jB?zA?fA?jB?hAAxA?lB?hA?nB?|A?hA?nB@jA@|ABnB@jA??BzABlBBhABxADjBBfADxAFjBDfADxAHhBDfAJfBFtAFbAJbB??HpAF`AL`BF~@HnAL`BH`AJnAL`BF`AL~AF~@JlAJxAHx@HfA??HlAFp@Fv@H`ADd@Dn@BXDVBN@D@B@@???@@@?B??DHHPFLJRNZLTL^Lf@B^Al@E\Kd@Mp@E`@Cj@Bx@??Bf@Ft@HbAFh@Np@Rt@P`@Vb@`@`@VLXHf@Fd@?l@?f@C`@A??l@Aj@Cn@Cv@Ip@If@Kp@Mp@Or@O~@Qt@Mj@K`ASj@Mv@QbAY??j@Qv@SbAUj@Mv@SbAUl@Mv@QdAWj@MbAWx@Qj@OdAUj@Ox@Q??bAUl@Ov@QbAWl@MdAUx@Ql@OdAWl@Ox@QbAUl@Ox@QbAWl@M??x@QbAUl@ObAUx@Sj@MbAUv@Qh@Mt@Qt@Qt@Q~@Sf@Mt@Q|@Q??f@Mp@O|@Sf@Mn@Qx@Qd@Kn@Ot@Qd@Kt@Q`@Kj@Mp@O`@Kf@M??n@O^Kf@Kp@O^Id@Mn@Q^Kf@On@S\Kl@Sf@O\Ml@S^M??b@Ol@SZMb@Mj@M\Ib@Gh@IZC`@Ed@CXA^?b@?V@Z@??`@BR@V?VILITYRSROZOTGVMVOTWZ_@PW^g@??TYZ_@Z_@`@c@Z_@VU\]b@a@XUZ]d@a@VWZ[b@a@TU`@_@??TUZW`@[VQZS`@WVOZO`@QVKZK^MTIXI^KRI??ZI\KTG^KXKTG^KXITG\KRGXI\KRGXI^K??RGXK\KRGZI\ITEXC\ERCXA\?R@V@\DP@??ZDPDTBVBNBRBTBL@N@RAJCNENAHDHL@R??ATGTGJIPGVEPIVMXIRMTM\IPKTOZGPKV??M\IRMXOZIRO\KPKXOZGPKTMXIPKVMZGP??KTMVIPKTOXEPIPGRCFAFEDCBEFEJGNIT??GNIPITELCJAN?FBLHNDLHNLRFLJPJTFL??JPLTHNHPDD@D@@????????????????????????@BDFDJFLDLHTHPHNLVHNNRNXHPJR??LRDLBNATEJIPKXIPKTOXKNOPKJOLQJMB??Q@Q?MAOAQAM?OAOCKAKEOCICGAE?CCCK???E?I?C??????????????????????????????????????????????????
```

You can find the source code for trip data post enrichment at:
<a href="/trip-schema/enriched.html">Enriched data</a>
