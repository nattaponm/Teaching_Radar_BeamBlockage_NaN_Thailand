# Teaching Radar–Terrain Beam Blockage with SRTM, wradlib & GIS

[![Python](https://img.shields.io/badge/Python-3.x-blue.svg)](https://www.python.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange.svg)](https://jupyter.org/)
[![Google Colab](https://img.shields.io/badge/Google-Colab-F9AB00.svg?logo=googlecolab&logoColor=white)](https://colab.research.google.com/)
[![wradlib](https://img.shields.io/badge/Radar-wradlib-1f77b4.svg)](https://docs.wradlib.org/)
[![Rasterio](https://img.shields.io/badge/GIS-Rasterio-139C5A.svg)](https://rasterio.readthedocs.io/)
[![SRTM DEM](https://img.shields.io/badge/DEM-SRTM-6B8E23.svg)](https://www.usgs.gov/centers/eros/science/usgs-eros-archive-digital-elevation-shuttle-radar-topography-mission-srtm)
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.22687421.svg)](https://doi.org/10.5281/zenodo.22687421)

> **แบบเรียนเชิงปฏิบัติการด้านภูมิศาสตร์เรดาร์และภูมิสารสนเทศ สำหรับทำความเข้าใจเรขาคณิตลำบีมเรดาร์ การบดบังจากภูมิประเทศ และการเปรียบเทียบพื้นที่ตั้งสถานีเรดาร์ด้วย Python, Google Colab, SRTM DEM และ wradlib**

Repository นี้พัฒนาขึ้นเพื่อใช้เป็นแบบเรียนแบบ **step-by-step** สำหรับนิสิตระดับปริญญาตรีชั้นปีที่ 3–4 และระดับบัณฑิตศึกษาในสาขา **ภูมิศาสตร์ ภูมิสารสนเทศ วิทยาศาสตร์สิ่งแวดล้อม อุตุนิยมวิทยา เรดาร์ตรวจอากาศ และสาขาที่เกี่ยวข้อง** โดยเน้นให้ผู้เรียนเข้าใจทั้ง **physical geography, radar geometry, raster GIS และ scientific interpretation** ไปพร้อมกัน

หลักสูตรสิ้นสุดที่ **Notebook 07 — Radar Site Decision Support and Common-Domain Spatial Evaluation** ครอบคลุมตั้งแต่การตรวจสอบและเตรียม SRTM DEM การฉายพิกัด การสร้าง radar geometry การคำนวณ Partial Beam Blockage (PBB) และ Cumulative Beam Blockage (CBB) การวิเคราะห์ตาม azimuth/range/elevation การประเมิน Minimum Usable Elevation (MUE) การเปรียบเทียบสถานีหลายแห่ง และการเปรียบเทียบแบบ **same-ground-cell** บน common Cartesian domain

เป้าหมายของหลักสูตรไม่ใช่เพียงให้ผู้เรียน “รันโค้ดได้” แต่ต้องสามารถตอบได้ว่า:

> **ภูมิประเทศที่ใช้มาจากไหน?  
> DEM ถูกแปลงอย่างไร?  
> ลำบีมเรดาร์อยู่สูงเท่าใด?  
> ภูเขาเริ่มตัดลำบีมที่ระยะใด?  
> PBB และ CBB ต่างกันอย่างไร?  
> elevation angle ที่สูงขึ้นช่วยแก้ blockage ได้เพียงใด?  
> และเมื่อเปรียบเทียบสถานีหลายแห่ง เรากำลังเปรียบเทียบพื้นที่เดียวกันจริงหรือไม่?**

---

## Course philosophy

แนวคิดหลักของแบบเรียนคือ

```text
SRTM Terrain Data
        ↓
Georeferencing / CRS
        ↓
DEM Reprojection & Resampling
        ↓
Radar-Site Geometry
        ↓
Radar Beam Geometry
        ↓
Terrain–Beam Intersection
        ↓
Partial Beam Blockage (PBB)
        ↓
Cumulative Beam Blockage (CBB)
        ↓
Azimuth / Range / Elevation Analysis
        ↓
Minimum Usable Elevation
        ↓
Multi-Site Comparison
        ↓
Common-Ground Spatial Evaluation
        ↓
Scientific Decision Support
```

ผู้เรียนจะไม่ได้เรียนเพียง “วิธีทำแผนที่ beam blockage” แต่จะเรียนรู้ว่า **การบดบังลำบีมเป็นผลจากปฏิสัมพันธ์ระหว่างภูมิประเทศกับเรขาคณิตของเรดาร์** ไม่ใช่คุณสมบัติของภูมิประเทศเพียงอย่างเดียว

หลักคิดที่ใช้ตลอดหลักสูตรคือ:

```text
Terrain alone ≠ beam blockage

DEM elevation ≠ radar antenna altitude

Fine raster grid ≠ fine radar observational resolution

PBB ≠ CBB

PBB at one gate ≠ cumulative visibility along the entire ray

Higher radar site ≠ automatically better radar site

Higher elevation angle ≠ automatically better low-level observation

Same polar-array index ≠ same ground location

Radar-range circle ≠ common-domain boundary

GIS overlay ≠ final engineering site approval
```

---

## Learning objectives

เมื่อเรียนครบ Notebook 00–07 ผู้เรียนควรสามารถ:

1. อธิบายแหล่งที่มา โครงสร้าง resolution, CRS, NoData และ metadata ของ SRTM DEM ได้
2. เข้าใจความสัมพันธ์ระหว่าง geographic coordinates กับ projected coordinates
3. อธิบายความหมายของ `.tif`, `.tfw`, affine transformation และ pixel center/pixel corner ได้
4. reproject DEM จาก EPSG:4326 ไปเป็น WGS84 / UTM Zone 47N (EPSG:32647) ได้
5. อธิบายเหตุผลของการใช้ maximum-elevation resampling สำหรับ terrain-obstruction screening ได้
6. อธิบายข้อจำกัดของการลดรายละเอียด DEM จากประมาณ 90 m ไปเป็น 1 km ได้
7. สร้าง radar polar geometry จาก azimuth, range และ elevation angle ได้
8. อธิบาย standard 4/3-Earth refraction model ได้
9. คำนวณ beam-center altitude และ half-power beam radius ได้
10. อธิบายและคำนวณ Partial Beam Blockage (PBB) ได้
11. อธิบายและคำนวณ Cumulative Beam Blockage (CBB) ได้
12. แยก clear, partial และ severe blockage ด้วย threshold ที่กำหนดเพื่อใช้ในการเปรียบเทียบได้
13. อธิบายว่าทำไม area-weighted statistics เหมาะสมกว่าการนับ polar gates ตรง ๆ ได้
14. วิเคราะห์ blockage ตาม range band และ azimuth ได้
15. สร้าง terrain–beam profile ตาม selected azimuth ได้
16. อธิบายความหมายของ lower beam edge, beam center และ upper beam edge ได้
17. คำนวณ D10 และ D50 เพื่อบอกระยะที่ blockage เริ่มมีนัยสำคัญได้
18. สร้าง Minimum Usable Elevation (MUE) map ได้
19. เปรียบเทียบ terrain visibility ของสถานีเรดาร์หลายตำแหน่งภายใต้ radar geometry เดียวกันได้
20. อธิบายข้อจำกัดของการเปรียบเทียบ polar arrays จากคนละ radar origin ได้
21. remap CBB ลง common Cartesian grid และเปรียบเทียบแบบ same-ground-cell ได้
22. สร้าง $\Delta CBB$ และ visibility-transition map ได้
23. แยกข้อสรุปทางวิทยาศาสตร์ออกจากข้อเสนอเชิงวิศวกรรมหรือการก่อสร้างจริงได้
24. สร้างรูป กราฟ แผนที่ และตารางที่ reproducible และเหมาะสำหรับต่อยอดงานวิจัยระดับวารสารได้

---

## Main scientific libraries

| Library | Role in this course | Link |
|---|---|---|
| **wradlib** | Radar geometry, half-power beam radius, PBB, CBB และ georeferencing | https://docs.wradlib.org/ |
| **Rasterio** | อ่าน/เขียน GeoTIFF, affine transform, raster sampling, reprojection | https://rasterio.readthedocs.io/ |
| **PyProj** | CRS และ coordinate transformation | https://pyproj4.github.io/pyproj/ |
| **NumPy** | Numerical arrays, masks, beam/range calculations | https://numpy.org/ |
| **Pandas** | Metadata, inventory, decision tables และ summary statistics | https://pandas.pydata.org/ |
| **Matplotlib** | Scientific figures, profiles, maps และ publication-ready visualization | https://matplotlib.org/ |
| **SciPy** | Spatial nearest-neighbour / KD-tree utilities ใน common-grid analysis | https://scipy.org/ |

Notebook แต่ละไฟล์ติดตั้ง package ที่จำเป็นใน Google Colab โดยตรง ผู้เรียนจึงไม่จำเป็นต้องติดตั้ง Python environment ในเครื่องก่อนเริ่มเรียน

---

# Data used in the course

## 1. SRTM Digital Elevation Model

แบบเรียนใช้ข้อมูล **Shuttle Radar Topography Mission (SRTM)** เป็นฐานข้อมูลภูมิประเทศสำหรับวิเคราะห์การบดบังลำบีมเรดาร์

Source information:

```text
Dataset family   : Shuttle Radar Topography Mission (SRTM)
Source agency    : NASA / USGS EROS
Source CRS       : EPSG:4326 — WGS 84 geographic coordinates
Native spacing   : approximately 3 arc-seconds (~90 m)
Vertical datum   : EGM96 geoid-based elevation
Elevation unit   : metre
Teaching archive : Zenodo DOI 10.5281/zenodo.22687421
```

USGS SRTM information:

https://www.usgs.gov/centers/eros/science/usgs-eros-archive-digital-elevation-shuttle-radar-topography-mission-srtm

Teaching dataset:

https://doi.org/10.5281/zenodo.22687421

ไฟล์ที่จัดเก็บใน Zenodo ประกอบด้วย source tiles ที่ใช้ครอบคลุมพื้นที่ภาคเหนือและบริเวณที่เกี่ยวข้องกับการวิเคราะห์สถานีเรดาร์จังหวัดน่าน ได้แก่:

```text
srtm_56_08.tif
srtm_56_09.tif
srtm_56_10.tif
srtm_57_08.tif
srtm_57_09.tif
srtm_57_10.tif
```

พร้อมไฟล์ประกอบ เช่น `.tfw` และ `.hdr`

> **Important:** source SRTM มีรายละเอียดสูงกว่า DEM 1 km ที่ใช้ในหลายขั้นของแบบเรียน การลด resolution มีวัตถุประสงค์เพื่อให้การวิเคราะห์ radar beam ใน Google Colab มีประสิทธิภาพและ reproducible ไม่ได้หมายความว่าภูมิประเทศจริงมี resolution 1 km

---

## 2. Radar-site study area

กรณีศึกษาหลักเป็นการเปรียบเทียบสถานีเรดาร์ปัจจุบันกับ candidate sites ในจังหวัดน่าน

| ID | Status | Location | Latitude | Longitude |
|---|---|---|---:|---:|
| **CURRENT** | Existing | Tha Wang Pha, Nan | 19.123087 | 100.813298 |
| **CAND_A** | Candidate | Tha Nao, Phu Phiang, Nan | 18.730690 | 100.815841 |
| **CAND_B** | Candidate | Woranakhon, Pua, Nan | 19.150692 | 100.925929 |
| **CAND_C** | Candidate | Phu Kha, Pua, Nan | 19.197099 | 101.078133 |

Candidate sites ใช้ใน repository นี้สำหรับ **teaching, terrain screening และ comparative radar-siting analysis** เท่านั้น ไม่ควรตีความว่าเป็นการรับรองความเหมาะสมด้านวิศวกรรมหรือการก่อสร้างจริง

---

## 3. Common radar geometry used in the course

เพื่อให้การเปรียบเทียบสถานีมีความเป็นธรรม แบบเรียนใช้ radar geometry เดียวกัน:

```text
Number of rays          : 360
Azimuth spacing         : 1°
Number of range bins    : 480
Range resolution        : 500 m
Maximum nominal range   : 240 km
Beamwidth               : 1.0°
Elevation angles        : 0.5°, 1.0°, 1.5°, 2.0°
Standard refraction     : k = 4/3
```

ดังนั้นหนึ่ง polar sweep เชิงเรขาคณิตมีขนาดประมาณ

$$
360\times480
$$

gates

และ maximum nominal range คือ

$$
480\times500\ \mathrm{m}
=
240\ \mathrm{km}
$$

---

# Core theory

## 1. Geographic coordinates, projected coordinates and CRS

SRTM source tiles อยู่ใน geographic coordinates:

$$
(\lambda,\phi)
$$

หรือ longitude–latitude ในหน่วย degree

แต่การวิเคราะห์ range, beam radius, distance และพื้นที่ต้องการหน่วย metric จึง reproject ไปเป็น

```text
EPSG:32647
WGS 84 / UTM Zone 47N
```

ใน UTM:

$$
(x,y)
$$

มีหน่วย metre ทำให้สามารถคำนวณระยะและพื้นที่ได้โดยตรง

**Key idea:** degree เป็นหน่วยเชิงมุม ไม่ใช่หน่วยระยะทางคงที่บนพื้นโลก

---

## 2. GeoTIFF, World File and affine transformation

Raster ไม่ได้เป็นเพียง matrix ของค่า elevation แต่ต้องรู้ว่า pixel แต่ละตำแหน่งอยู่ที่ใดบนโลก

ความสัมพันธ์พื้นฐานของ affine transformation เขียนได้เป็น

$$
x = A c + B r + C
$$

$$
y = D c + E r + F
$$

เมื่อ

- $c$ = column
- $r$ = row
- $A$ = pixel size ตามแกน X
- $E$ = pixel size ตามแกน Y
- $B,D$ = rotation terms
- $C,F$ = translation/origin terms

สำหรับ north-up raster โดยทั่วไป $E$ เป็นค่าลบเพราะ row index เพิ่มลงด้านล่างของ array

ข้อสำคัญคือ TIFF World File มักเก็บตำแหน่ง **pixel center** ขณะที่ GDAL/Rasterio affine transform ใช้ **upper-left pixel corner** เป็น reference จึงต้องระวัง half-pixel offset

**Key idea:** raster ที่ค่า elevation ถูกต้องแต่ georeferencing ผิด สามารถทำให้ terrain–beam interaction เลื่อนตำแหน่งทั้งระบบได้

---

## 3. Why reproject to UTM?

การคำนวณ beam blockage ต้องเชื่อม

```text
Radar location
+
Range
+
Azimuth
+
Terrain elevation
```

บนพื้นที่เดียวกัน

ถ้ายังอยู่ใน latitude/longitude การคำนวณระยะและพื้นที่จะไม่สม่ำเสมอ จึงใช้ UTM Zone 47N เพื่อให้

$$
1\ \mathrm{grid\ cell}
=
1000\ \mathrm{m}
\times
1000\ \mathrm{m}
$$

ใน DEM ที่ resample เป็น 1 km

---

## 4. DEM resampling and maximum elevation

เมื่อลด source SRTM จากประมาณ 90 m ไปเป็น 1 km มีหลายวิธี เช่น mean, bilinear หรือ maximum

แบบเรียนใช้ **maximum elevation** เป็นค่าเริ่มต้น:

$$
z_{1km}
=
\max(z_{\mathrm{native}})
$$

เหตุผลคือ ridge แคบอาจเป็น terrain obstacle ที่สำคัญต่อ radar beam ถ้าใช้ mean หรือ bilinear ridge อาจถูก smooth และทำให้ประเมิน blockage ต่ำเกินจริง

อย่างไรก็ตาม:

> **1-km MAX DEM เป็น conservative terrain representation สำหรับ regional screening ไม่ใช่ replacement ของ high-resolution DEM สำหรับ final site engineering**

และไม่ควรใช้ 1-km MAX cell เป็น radar antenna altitude โดยตรง

---

## 5. Radar altitude

ควรแยก

```text
terrain elevation
radar-site ground elevation
antenna height above ground
radar feed altitude
```

ออกจากกัน

โดย

$$
h_0
=
z_{\mathrm{site}}
+
H_{\mathrm{antenna}}
$$

เมื่อ

- $z_{\mathrm{site}}$ = ground elevation ที่ตำแหน่ง radar
- $H_{\mathrm{antenna}}$ = antenna/feed height above local ground
- $h_0$ = radar feed altitude MSL

ในแบบเรียนใช้ native-resolution SRTM ที่ตำแหน่งสถานีเพื่อประมาณ $z_{\mathrm{site}}$ แล้วจึงเพิ่ม antenna-height assumption

---

## 6. Effective Earth radius and beam-center height

เนื่องจาก Earth curvature และ atmospheric refraction ลำบีมเรดาร์ไม่ได้เดินเป็นเส้นตรงเทียบกับพื้นโลกแบบ Cartesian อย่างง่าย

ภายใต้ standard-refraction approximation ใช้

$$
k=\frac{4}{3}
$$

และ effective Earth radius คือ

$$
R_e^\ast
=
kR_e
$$

beam-center altitude โดยประมาณเป็น

$$
h_b(r,\theta)
=
\sqrt{
r^2+
(kR_e)^2+
2rkR_e\sin\theta
}
-
kR_e
+
h_0
$$

เมื่อ

- $r$ = slant range
- $\theta$ = elevation angle
- $R_e$ = Earth radius
- $k$ = effective-Earth-radius factor
- $h_0$ = radar feed altitude MSL

จากสมการนี้จะเห็นว่า beam สูงขึ้นเมื่อ

```text
range increases
or
elevation angle increases
```

ดังนั้น radar ไม่ได้ตรวจชั้นบรรยากาศที่ระดับเดียวกันตลอด 240 km

---

## 7. Beam width and half-power radius

ถ้า beamwidth เท่ากับ $\beta$ โดยประมาณ half-power beam radius เป็น

$$
a(r)
\approx
r
\tan
\left(
\frac{\beta}{2}
\right)
$$

ดังนั้น beam กว้างขึ้นตามระยะทาง

นี่เป็นเหตุผลหนึ่งที่ observational resolution ของ weather radar ลดลงเมื่อระยะห่างจาก radar เพิ่มขึ้น

**Key idea:** 500-m gate spacing ไม่ได้หมายความว่า effective spatial resolution ของ beam เท่ากับ 500 m ทุกระยะ

---

## 8. Partial Beam Blockage (PBB)

กำหนด

$$
y
=
z_t-z_b
$$

เมื่อ

- $z_t$ = terrain elevation
- $z_b$ = beam-center altitude
- $a$ = half-power beam radius

สำหรับกรณีที่ terrain ตัดวงหน้าตัดของ beam บางส่วน PBB สามารถเขียนในรูป normalized circular-segment relation ได้เป็น

$$
PBB
=
\frac{1}{\pi}
\left[
\arcsin
\left(
\frac{y}{a}
\right)
+
\frac{y}{a}
\sqrt{
1-
\left(
\frac{y}{a}
\right)^2
}
+
\frac{\pi}{2}
\right]
$$

สำหรับ

$$
-a<y<a
$$

และมีขอบเขต

$$
PBB=0
\qquad
\text{เมื่อ }y\le-a
$$

$$
PBB=1
\qquad
\text{เมื่อ }y\ge a
$$

ดังนั้น

```text
PBB = 0   → beam cross-section ไม่ถูก terrain บัง
PBB = 1   → beam cross-section ถูก terrain บังเต็ม
```

ใน repository ใช้ implementation ของ `wradlib.qual.beam_block_frac()` เพื่อคำนวณ PBB

---

## 9. Cumulative Beam Blockage (CBB)

PBB บอกการบดบังที่ gate ปัจจุบัน แต่ถ้าภูเขาบัง beam ตั้งแต่ระยะใกล้ การมองเห็นด้านหลังภูเขาย่อมได้รับผลต่อเนื่อง

จึงใช้ CBB:

$$
CBB(r_i)
=
\max_{j\le i}
PBB(r_j)
$$

หรือกล่าวได้ว่า CBB เป็น running maximum ของ blockage ตามแนว ray

ตัวอย่าง:

```text
Range       PBB       CBB
20 km       0.00      0.00
30 km       0.20      0.20
40 km       0.65      0.65
50 km       0.10      0.65
100 km      0.00      0.65
```

แม้ terrain ที่ 100 km ไม่ตัด beam โดยตรง แต่ ray ถูกบังไปแล้ว 65% จาก obstacle ที่ 40 km

ใน repository ใช้ `wradlib.qual.cum_beam_block_frac()`

---

## 10. Teaching blockage classes

เพื่อใช้ในการเปรียบเทียบ แบบเรียนกำหนด threshold เชิงการสอน:

```text
Clear / essentially unblocked : CBB < 0.10
Partial blockage              : 0.10 ≤ CBB < 0.50
Severe blockage               : CBB ≥ 0.50
```

threshold เหล่านี้ใช้เป็น **diagnostic classes** เพื่อให้เปรียบเทียบสถานีและ elevation angles ได้ง่าย ไม่ควรตีความเป็น operational certification threshold โดยอัตโนมัติ

---

## 11. Area weighting in polar coordinates

polar gates ที่ระยะไกลครอบคลุมพื้นที่มากกว่า gates ที่ระยะใกล้

พื้นที่ sector ระหว่าง $r_1$ และ $r_2$ คือ

$$
A
=
\frac{1}{2}
\left(
r_2^2-r_1^2
\right)
\Delta\alpha
$$

เมื่อ $\Delta\alpha$ อยู่ในหน่วย radian

ดังนั้นการนับจำนวน gates ตรง ๆ อาจให้น้ำหนัก near-range มากเกินไป

แบบเรียนจึงใช้ **area-weighted statistics** สำหรับ clear/partial/severe coverage

**Key idea:** gate count และ ground area ไม่ใช่สิ่งเดียวกัน

---

## 12. D10 and D50

เพื่อบอกว่าการบดบังเริ่มมีนัยสำคัญที่ระยะใด นิยาม

$$
D_{10}
=
\min
\left\{
r:
CBB(r)\ge0.10
\right\}
$$

และ

$$
D_{50}
=
\min
\left\{
r:
CBB(r)\ge0.50
\right\}
$$

ความหมาย:

```text
Small D10 → terrain เริ่มมีผลต่อ beam ตั้งแต่ระยะใกล้

Small D50 → severe blockage เกิดตั้งแต่ระยะใกล้

No D50 within 240 km → ray ไม่ถึง severe-blockage threshold ภายใน analysis range
```

---

## 13. Minimum Usable Elevation (MUE)

ให้ชุด elevation angles เป็น

$$
E
=
\{
0.5^\circ,
1.0^\circ,
1.5^\circ,
2.0^\circ
\}
$$

Minimum Usable Elevation ที่ตำแหน่งหนึ่งนิยามเป็น

$$
MUE(x,y)
=
\min
\left\{
\theta\in E:
CBB_\theta(x,y)<0.10
\right\}
$$

ถ้าไม่มี elevation ใดผ่าน threshold ให้จัดเป็น

```text
Not clear at 0.5–2.0°
```

MUE จึงตอบคำถามได้ง่ายกว่า CBB maps หลายภาพว่า:

> “ที่ ground location นี้ ต้องใช้ elevation อย่างน้อยเท่าใดจึงจะมี terrain visibility ตามเกณฑ์ที่กำหนด?”

---

## 14. Common spatial domain

radar แต่ละสถานีมี origin ต่างกัน ดังนั้น polar array index เดียวกัน เช่น

```text
azimuth = 90°
range = 100 km
```

ไม่ได้หมายถึงตำแหน่งบนพื้นโลกเดียวกัน

เพื่อเปรียบเทียบอย่างยุติธรรม Notebook 07 สร้าง common Cartesian grid และกำหนด

$$
D_{\mathrm{common}}
=
\bigcap_{i=1}^{n}D_i
$$

เมื่อ $D_i$ คือพื้นที่ที่อยู่ภายใน radar range ของ site ที่ $i$

นี่เป็นเหตุผลที่ **common-domain boundary ไม่จำเป็นต้องเป็นวงกลม**

---

## 15. Same-ground-cell CBB difference

เมื่อ CBB ของสอง radar ถูก remap ลง common grid แล้ว สามารถคำนวณ

$$
\Delta CBB(x,y)
=
CBB_{\mathrm{PhuPhiang}}(x,y)
-
CBB_{\mathrm{Current}}(x,y)
$$

ถ้า

$$
\Delta CBB<0
$$

หมายถึง Phu Phiang มี terrain-induced blockage ต่ำกว่าที่ **ground cell เดียวกัน**

ถ้า

$$
\Delta CBB>0
$$

หมายถึง current site มี blockage ต่ำกว่า

---

# Visualization standards

เพื่อให้ figures ใช้ได้ทั้งในการเรียนและต่อยอดสู่ publication workflow แบบเรียนกำหนดมาตรฐานคงที่ดังนี้

## DEM

ใช้ colormap:

```python
cmap="terrain"
```

Colorbar:

```text
Terrain elevation (m MSL)
```

## Beam blockage

PBB/CBB ใช้:

```python
cmap="PuRd"
vmin=0
vmax=1
```

เพื่อให้ทุก notebook อ่านค่า blockage ด้วย scale เดียวกัน

## Radar-site symbols

```text
Current radar site   → orange star with black edge
Candidate radar site → white triangle with black edge
```

หลีกเลี่ยง marker สีน้ำเงินบน DEM เพราะอาจกลืนกับพื้นที่ elevation ต่ำใน `terrain` colormap

## Maps

- ใช้ projected coordinates เมื่อวิเคราะห์ระยะทาง
- แกน UTM แสดงเป็น km เพื่ออ่านง่าย
- ใช้ `ax.set_aspect("equal")`
- ป้องกัน label overlap
- colorbar ต้องมี physical unit
- figure title และ axis labels ใช้ภาษาอังกฤษเพื่อให้นำไปต่อยอด publication ได้ง่าย
- export figures อย่างน้อยประมาณ 300 dpi

## Critical cartographic distinction

```text
Radar-range circle ≠ common-domain boundary
```

ใน Notebook 07:

- dotted circle = nominal radar range
- solid/dashed boundary = intersection-based common domain
- colored CBB field = values restricted to the specified comparison domain

---

# Course workflow

```text
00  SRTM Dataset / Zenodo / Metadata Audit
 │
01  DEM Preprocessing / CRS / UTM / 1-km MAX
 │
02  Radar Geometry / PBB / CBB / 4 Sites × 4 Elevations
 │
03  Phu Phiang Candidate / Terrain Horizon / MUE / Sensitivity
 │
04  Selected-Azimuth Terrain–Beam Profiles
 │
05  Phu Phiang Multi-Elevation Directional Diagnostics
 │
06  Multi-Site Comparison / MUE / Area-Weighted Coverage
 │
07  Common Cartesian Domain / Same-Ground-Cell Decision Support
```

อีกมุมหนึ่งสามารถมองเป็นสามช่วงการเรียน:

```text
FOUNDATION
00 → 01 → 02

PHYSICAL INTERPRETATION
03 → 04 → 05

COMPARATIVE GIS / DECISION SUPPORT
06 → 07
```

---

# Notebooks

> Repository: `nattaponm/Teaching_Radar_BeamBlockage_NaN_Thailand`  
> Notebook filenames and GitHub/Colab links below follow the current `main` branch.

<table>
<thead>
<tr>
<th>No.</th>
<th>Notebook</th>
<th>Purpose</th>
<th>Core theory / concepts</th>
<th>What students should examine</th>
<th>Open</th>
</tr>
</thead>
<tbody>

<tr>
<td><b>00</b></td>
<td><b>SRTM Dataset Acquisition, Zenodo & Metadata Audit</b><br><sub>00_Setup_and_Download_SRTM_DEM.ipynb</sub></td>
<td>ดาวน์โหลด teaching DEM archive จาก Zenodo ตรวจไฟล์ source tiles, GeoTIFF/TFW/HDR, metadata, file size, CRS, resolution, NoData และ provenance ก่อนเริ่ม preprocessing</td>
<td>SRTM provenance, raster metadata, GeoTIFF, TFW, affine transformation, pixel centre/corner, EPSG:4326, data integrity</td>
<td>source tiles ครบหรือไม่, georeferencing สมเหตุสมผลหรือไม่, resolution ประมาณ 3 arc-second หรือไม่, NoData และ elevation range สมเหตุสมผลหรือไม่</td>
<td><a href="https://github.com/nattaponm/Teaching_Radar_BeamBlockage_NaN_Thailand/blob/main/00_Setup_and_Download_SRTM_DEM.ipynb">GitHub</a><br>
<a href="https://colab.research.google.com/github/nattaponm/Teaching_Radar_BeamBlockage_NaN_Thailand/blob/main/00_Setup_and_Download_SRTM_DEM.ipynb"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"></a></td>
</tr>

<tr>
<td><b>01</b></td>
<td><b>Prepare 1-km SRTM DEM Mosaic</b><br><sub>01_SRTM_DEM_Preprocessing_for_Radar_Terrain_Analysis.ipynb</sub></td>
<td>เตรียม DEM ที่เหมาะกับการคำนวณ radar beam blockage โดยคัด source tiles, reproject ไป UTM Zone 47N และ resample เป็น 1 km ด้วย maximum elevation</td>
<td>CRS, EPSG:4326 → EPSG:32647, raster reprojection, resampling, maximum elevation, NoData, radar-domain coverage</td>
<td>ทำไมต้องใช้ projected CRS, ทำไมใช้ MAX แทน mean, ความต่างระหว่าง native site elevation กับ coarse 1-km MAX cell และ coverage ของ DEM ภายใน 240 km</td>
<td><a href="https://github.com/nattaponm/Teaching_Radar_BeamBlockage_NaN_Thailand/blob/main/01_SRTM_DEM_Preprocessing_for_Radar_Terrain_Analysis.ipynb">GitHub</a><br>
<a href="https://colab.research.google.com/github/nattaponm/Teaching_Radar_BeamBlockage_NaN_Thailand/blob/main/01_SRTM_DEM_Preprocessing_for_Radar_Terrain_Analysis.ipynb"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"></a></td>
</tr>

<tr>
<td><b>02</b></td>
<td><b>Nan Radar Beam Blockage — 4 Sites × 4 Elevations</b><br><sub>02_Radar_Beam_Geometry_for_Terrain_Analysis.ipynb</sub></td>
<td>สร้าง radar geometry และคำนวณ PBB/CBB ของ current radar และ candidate sites ทั้ง 4 ตำแหน่งที่ elevation 0.5°, 1.0°, 1.5° และ 2.0° ภายใต้เงื่อนไขเดียวกัน</td>
<td>4/3-Earth model, beam height, half-power radius, PBB, CBB, polar gate area, blockage classes, range/azimuth comparison</td>
<td>site ใดถูก terrain บังมากที่สุด, blockage เปลี่ยนอย่างไรเมื่อเพิ่ม elevation, ทำไม PBB กับ CBB ให้ความหมายต่างกัน และเหตุใด area weighting จึงสำคัญ</td>
<td><a href="https://github.com/nattaponm/Teaching_Radar_BeamBlockage_NaN_Thailand/blob/main/02_Radar_Beam_Geometry_for_Terrain_Analysis.ipynb">GitHub</a><br>
<a href="https://colab.research.google.com/github/nattaponm/Teaching_Radar_BeamBlockage_NaN_Thailand/blob/main/02_Radar_Beam_Geometry_for_Terrain_Analysis.ipynb"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"></a></td>
</tr>

<tr>
<td><b>03</b></td>
<td><b>Phu Phiang Candidate — Terrain–Beam Visibility Decision Support</b><br><sub>03_Terrain_Sampling_and_Terrain_Beam_Profiles.ipynb</sub></td>
<td>เจาะลึก candidate หลักที่ตำบลท่าน้าว อำเภอภูเพียง โดยใช้ terrain horizon, hybrid DEM, MUE, first-blockage distance, lower-beam clearance และ sensitivity concepts</td>
<td>terrain horizon, hybrid DEM, near-field vs regional DEM, minimum usable elevation, first blockage, lower-beam clearance, tower-height sensitivity</td>
<td>ทิศใดเป็น critical terrain sectors, elevation ใดพ้นภูเขา, DEM resolution มีผลต่อ near-field blockage หรือไม่ และ Phu Phiang มีลักษณะ terrain setting อย่างไร</td>
<td><a href="https://github.com/nattaponm/Teaching_Radar_BeamBlockage_NaN_Thailand/blob/main/03_Terrain_Sampling_and_Terrain_Beam_Profiles.ipynb">GitHub</a><br>
<a href="https://colab.research.google.com/github/nattaponm/Teaching_Radar_BeamBlockage_NaN_Thailand/blob/main/03_Terrain_Sampling_and_Terrain_Beam_Profiles.ipynb"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"></a></td>
</tr>

<tr>
<td><b>04</b></td>
<td><b>Selected-Azimuth Terrain–Beam Blockage Profiles</b><br><sub>04_Partial_and_Cumulative_Beam_Blockage_with_wradlib.ipynb</sub></td>
<td>แสดงปฏิสัมพันธ์ terrain–beam ตาม selected azimuth 15°, 80° และ 210° พร้อม profile ของ terrain, beam centre, ±3-dB beam edges, PBB และ CBB</td>
<td>selected ray, vertical geometry, half-power beam envelope, critical ridge, D10, D50, directional obstruction</td>
<td>ridge ใดควบคุม blockage, beam lower edge อยู่เหนือหรือต่ำกว่า terrain เมื่อใด, D10/D50 เปลี่ยนตาม elevation อย่างไร และเหตุใด obstruction ใกล้ radar จึงสำคัญมาก</td>
<td><a href="https://github.com/nattaponm/Teaching_Radar_BeamBlockage_NaN_Thailand/blob/main/04_Partial_and_Cumulative_Beam_Blockage_with_wradlib.ipynb">GitHub</a><br>
<a href="https://colab.research.google.com/github/nattaponm/Teaching_Radar_BeamBlockage_NaN_Thailand/blob/main/04_Partial_and_Cumulative_Beam_Blockage_with_wradlib.ipynb"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"></a></td>
</tr>

<tr>
<td><b>05</b></td>
<td><b>Phu Phiang Multi-Elevation Directional Diagnostics</b><br><sub>05_Multi_Elevation_Beam_Blockage_and_Radar_Coverage.ipynb</sub></td>
<td>สังเคราะห์ CBB ของ Phu Phiang ในทุก azimuth และ elevation ด้วย D10, D50, directional maxima, range-band clear coverage, ΔCBB และ MUE</td>
<td>D10/D50 by azimuth, area-weighted clear coverage, elevation dependence, directional CBB, MUE, beam altitude by range</td>
<td>azimuth sectors ใดฟื้น visibility เมื่อเพิ่ม elevation, 1.5° กับ 2.0° ต่างกันอย่างไร, low-elevation coverage เสียไปบริเวณใด และ beam สูงขึ้นมากเพียงใดที่ระยะไกล</td>
<td><a href="https://github.com/nattaponm/Teaching_Radar_BeamBlockage_NaN_Thailand/blob/main/05_Multi_Elevation_Beam_Blockage_and_Radar_Coverage.ipynb">GitHub</a><br>
<a href="https://colab.research.google.com/github/nattaponm/Teaching_Radar_BeamBlockage_NaN_Thailand/blob/main/05_Multi_Elevation_Beam_Blockage_and_Radar_Coverage.ipynb"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"></a></td>
</tr>

<tr>
<td><b>06</b></td>
<td><b>Multi-Site MUE and Area-Weighted Coverage Comparison</b><br><sub>06_Multi_Site_Radar_Terrain_Visibility_Comparison.ipynb</sub></td>
<td>เปรียบเทียบ Current Tha Wang Pha, Phu Phiang, Woranakhon และ Phu Kha ด้วย metric เดียวกัน ทั้ง MUE, fraction of azimuths, clear/partial/severe area และ improvement relative to current site</td>
<td>multi-site comparability, MUE distribution, area weighting, clear-area improvement, severe-area reduction, elevation-dependent radar visibility</td>
<td>candidate ใดดีกว่าปัจจุบันอย่างสม่ำเสมอ, candidate ใดมีผลใกล้เคียง current, high site สามารถแย่ได้เพราะอะไร และการเพิ่ม elevation ช่วยแต่ละ site ต่างกันอย่างไร</td>
<td><a href="https://github.com/nattaponm/Teaching_Radar_BeamBlockage_NaN_Thailand/blob/main/06_Multi_Site_Radar_Terrain_Visibility_Comparison.ipynb">GitHub</a><br>
<a href="https://colab.research.google.com/github/nattaponm/Teaching_Radar_BeamBlockage_NaN_Thailand/blob/main/06_Multi_Site_Radar_Terrain_Visibility_Comparison.ipynb"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"></a></td>
</tr>

<tr>
<td><b>07</b></td>
<td><b>Radar Site Decision Support & Common-Domain Spatial Evaluation</b><br><sub>07_Radar_Site_Decision_Support_and_Common_Domain_Spatial_Evaluation.ipynb</sub></td>
<td>เปลี่ยนจากการเปรียบเทียบ polar arrays ไปเป็น same-ground-cell comparison บน common 1-km Cartesian grid สร้าง ΔCBB, visibility-transition map, common-domain MUE และ evidence matrix</td>
<td>common spatial domain, Cartesian remapping, same-ground-cell comparison, ΔCBB, visibility gain/loss, spatial comparability, radar range vs intersection boundary</td>
<td>เมื่อเปรียบเทียบพื้นที่เดียวกันจริง Phu Phiang ยังได้เปรียบหรือไม่, gain เกิดในพื้นที่ใด, loss เกิดในพื้นที่ใด, common domain ต่างจาก radar-range circle อย่างไร และผลลัพธ์ควรใช้สนับสนุนการตัดสินใจในระดับใด</td>
<td><a href="https://github.com/nattaponm/Teaching_Radar_BeamBlockage_NaN_Thailand/blob/main/07_Radar_Site_Decision_Support_and_Common_Domain_Spatial_Evaluation.ipynb">GitHub</a><br>
<a href="https://colab.research.google.com/github/nattaponm/Teaching_Radar_BeamBlockage_NaN_Thailand/blob/main/07_Radar_Site_Decision_Support_and_Common_Domain_Spatial_Evaluation.ipynb"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"></a></td>
</tr>

</tbody>
</table>

---

# Concept map by notebook

## 00 — SRTM Dataset, Zenodo and metadata audit

### Why this notebook?

ก่อนคำนวณ beam blockage ผู้เรียนต้องมั่นใจก่อนว่า DEM ที่ใช้มีที่มาและ georeferencing ถูกต้อง

Notebook 00 จึงทำหน้าที่เป็น **data-provenance and readiness notebook**

### Scientific concepts

```text
Data provenance
GeoTIFF
TFW / World File
Raster dimensions
Pixel size
CRS
NoData
Elevation range
Zenodo DOI
```

### Before running

ผู้เรียนควรตอบได้ก่อนว่า:

- SRTM คือข้อมูลชนิดใด
- elevation เป็น horizontal position หรือ vertical quantity
- CRS และ vertical datum เป็นคนละสิ่งกันอย่างไร
- `.tfw` มีหน้าที่อะไร
- ทำไม metadata ต้องตรวจตั้งแต่ต้น

### What the code does

```text
Zenodo record
      ↓
Download archive
      ↓
Extract files
      ↓
Inventory
      ↓
Read GeoTIFF metadata
      ↓
Check TFW/HDR
      ↓
Plot source-tile coverage
      ↓
Quick-look DEM
      ↓
Save provenance + inventory
```

### Expected outputs

- source tile inventory
- Zenodo provenance JSON
- tile-coverage map
- DEM quick-look
- metadata CSV
- readiness report

### Scientific interpretation

Notebook นี้ยัง **ไม่คำนวณ radar beam** หน้าที่ของมันคือยืนยันว่า terrain dataset พร้อมสำหรับการวิเคราะห์

**Key idea:** *Data provenance precedes physical modelling.*

---

## 01 — Prepare SRTM 1-km mosaic

Notebook 01 สอน GIS preprocessing ที่จำเป็นสำหรับ radar terrain analysis

source DEM:

$$
EPSG:4326
$$

ถูกแปลงไปเป็น:

$$
EPSG:32647
$$

และ resample เป็น:

$$
1000\times1000\ \mathrm{m}
$$

### Why use UTM?

เพราะในบทต่อไปเราจะคำนวณ

```text
range
beam radius
ground distance
area
```

ทั้งหมดในหน่วย metre

### Why MAX resampling?

เพื่อรักษา ridge crests:

$$
z_{\mathrm{cell}}
=
\max(z_i)
$$

แทนที่จะใช้

$$
\bar z
$$

ซึ่งอาจลดความสูงของ ridge

### What students should examine

- source vs destination CRS
- source vs output resolution
- output bounds
- NoData coverage
- native site elevation
- 1-km MAX elevation at the same site
- difference between native and coarse elevation

**Key idea:** *DEM processing choices can change the estimated terrain obstruction.*

---

## 02 — 4 Sites × 4 Elevations

นี่คือบทหลักของ radar beam-blockage physics

workflow:

```text
Radar location
      ↓
Azimuth × Range × Elevation
      ↓
Beam geometry
      ↓
Sample DEM at gates
      ↓
PBB
      ↓
CBB
      ↓
Classify
      ↓
Area statistics
      ↓
Maps
```

### Before running

ผู้เรียนควรเข้าใจ:

$$
h_b(r,\theta)
$$

$$
a(r)
$$

$$
PBB
$$

และ

$$
CBB
$$

### What to examine

อย่าดูเพียงค่า maximum CBB แต่ให้ดูพร้อมกันว่า

- blockage เกิด azimuth ใด
- เริ่มที่ range ใด
- กินพื้นที่เท่าใด
- เพิ่ม elevation แล้วดีขึ้นหรือไม่

### Expected figures

- DEM + radar sites
- PBB maps
- CBB maps
- clear/partial/severe coverage
- range-band statistics
- azimuth statistics
- comparative heatmaps
- worst-ray terrain profiles

**Key idea:** *A radar site should be evaluated in direction, range and elevation — not by one scalar number.*

---

## 03 — Phu Phiang terrain–beam visibility

Notebook 03 เลือก Phu Phiang เป็น candidate หลักเพื่อวิเคราะห์รายละเอียดมากขึ้น

แนวคิดสำคัญ:

```text
Terrain horizon
      ↓
Beam interception
      ↓
Cumulative blockage
      ↓
Operationally usable elevation
```

### Hybrid DEM

เพื่อหลีกเลี่ยงการยก terrain ใกล้ radar สูงเกินจริงจาก 1-km MAX DEM มีการใช้แนวคิด:

```text
Near field  : local finer DEM
Far field   : regional conservative DEM
```

นี่เป็นตัวอย่างที่ดีของ **scale-dependent geospatial modelling**

### Terrain horizon

แนวคิดอย่างง่ายคือ

$$
\alpha_t
=
\tan^{-1}
\left(
\frac{
z_t-z_0
}{
d
}
\right)
$$

แต่การใช้งานจริงควรพิจารณา Earth curvature และ radar geometry ร่วมด้วย

### What to examine

- ridge ที่กำหนด horizon
- first blockage distance
- MUE
- lower-beam clearance
- effect of DEM resolution
- effect of tower-height assumptions

**Key idea:** *Near-site topography can dominate the visibility of an entire downstream radar ray.*

---

## 04 — Selected-azimuth profiles

Notebook 04 เปลี่ยนจาก map view ไปเป็น vertical terrain–beam profile

selected azimuths:

```text
15°
80°
210°
```

elevation angles:

```text
0.5°
1.0°
1.5°
2.0°
```

รวม

$$
3\times4
=
12
$$

selected-ray scenarios

### Composite interpretation

หนึ่ง figure ควรอ่านพร้อมกันสามมิติ:

```text
Where is the ray?
What terrain does it cross?
Where is the beam vertically?
How much of the beam is blocked?
How does blockage persist downstream?
```

### Beam envelope

โดยประมาณ:

$$
z_{\mathrm{lower}}
=
z_b-a
$$

$$
z_{\mathrm{upper}}
=
z_b+a
$$

เมื่อ $a$ คือ half-power beam radius

ถ้า terrain สูงเข้าใกล้หรือสูงกว่า lower edge จะเริ่มมี partial blockage

### D10 / D50

Notebook นี้ทำให้ D10/D50 มีความหมายเชิงกายภาพ เพราะผู้เรียนสามารถมองเห็น ridge ที่เป็นสาเหตุได้โดยตรง

**Key idea:** *Profile analysis connects a map-based blockage index to the actual terrain obstacle.*

---

## 05 — Phu Phiang multi-elevation directional diagnostics

Notebook 05 ขยายจาก selected rays ไปสู่ azimuth ทั้ง 360°

### D10 by azimuth

$$
D_{10}(\alpha)
$$

บอกว่าทิศใด terrain เริ่มมีผลเร็ว

### D50 by azimuth

$$
D_{50}(\alpha)
$$

บอกว่าทิศใด severe blockage เริ่มใกล้ radar

### Range-band coverage

ตัวอย่าง bands:

```text
0–50 km
50–100 km
100–150 km
150–200 km
200–240 km
```

ทำให้แยกได้ว่า candidate มีข้อดีเฉพาะ near range หรือยังรักษาความได้เปรียบไปถึง far range

### Beam-altitude trade-off

การเพิ่ม elevation ลด terrain blockage ได้ แต่ทำให้ beam สูงขึ้น

จึงต้องคิดพร้อมกันว่า:

```text
less terrain blockage
does not necessarily mean
better low-level atmospheric sampling
```

**Key idea:** *Elevation angle is a trade-off between terrain clearance and sampling altitude.*

---

## 06 — Multi-site comparison

Notebook 06 สร้าง comparative evidence ของ 4 sites ภายใต้ radar geometry เดียวกัน

วิเคราะห์:

```text
Current: Tha Wang Pha
Phu Phiang
Woranakhon, Pua
Phu Kha high site
```

### Questions

- site ใดมี clear coverage มากกว่า
- site ใดต้องใช้ elevation สูงกว่า
- site ใดมี severe blockage ลดลงจริง
- site บนภูเขาสูงจำเป็นต้องดีที่สุดหรือไม่

ผลจากแบบเรียนแสดงให้เห็นแนวคิดสำคัญว่า:

> **High terrain elevation of a radar site does not automatically guarantee superior terrain visibility.**

เพราะ topographic setting รอบสถานีและ near-field ridges มีความสำคัญมาก

### MUE distribution

MUE ช่วยแปลง 4 CBB maps ให้เป็น map เดียวที่ตอบว่า elevation ต่ำสุดที่ usable ตาม threshold คือเท่าใด

**Key idea:** *Site elevation and site visibility are related but not equivalent.*

---

## 07 — Common-domain spatial evaluation

นี่คือ capstone ของหลักสูตร

ปัญหาคือ radar แต่ละ site มี origin ต่างกัน

ดังนั้น

```text
array[azimuth=100°, range=150 km]
```

ของ Phu Phiang กับ Tha Wang Pha ไม่ได้อยู่ geographic location เดียวกัน

Notebook 07 จึงสร้าง **common Cartesian grid**

### Same-ground-cell concept

หลัง remapping:

$$
CBB_A(x,y)
$$

และ

$$
CBB_B(x,y)
$$

อยู่บน ground cells เดียวกัน

จึงเปรียบเทียบได้โดยตรง:

$$
\Delta CBB(x,y)
=
CBB_A(x,y)
-
CBB_B(x,y)
$$

### Visibility transition

สำหรับ threshold

$$
CBB<0.10
$$

หนึ่ง ground cell สามารถจัดเป็น:

```text
Neither clear
Both clear
Gain at Phu Phiang
Loss at Phu Phiang
```

นี่เป็น product ที่เหมาะสำหรับ decision support เพราะแสดง **พื้นที่ที่ได้ประโยชน์จากการย้าย radar** แทนที่จะรายงานเพียงค่าเฉลี่ยรวม

### Common domain geometry

ถ้า radar ranges เป็น $D_1,D_2,\ldots,D_n$

$$
D_{\mathrm{common}}
=
D_1\cap D_2\cap\cdots\cap D_n
$$

ดังนั้น boundary ของ common domain ไม่จำเป็นต้องเป็น circle

**Key idea:** *Spatial comparability must be established before site-to-site differences are interpreted.*

---

# Recommended way to study

## Undergraduate Year 3–4

แนะนำให้เรียนทุก Notebook แต่เน้นแนวคิดดังนี้:

```text
00 → Data provenance
01 → Raster GIS / CRS
02 → PBB / CBB physics
03 → Terrain interpretation
04 → Profiles
05 → Direction / range
06 → Comparative reasoning
07 → GIS decision support
```

เป้าหมายหลักคือให้ผู้เรียน **อธิบายได้** มากกว่าปรับ parameter จำนวนมาก

ผู้สอนอาจให้ผู้เรียนเลือกเพียง:

- 1 candidate site
- 1–2 elevation angles
- 1–2 selected azimuths

ในแบบฝึกหัดย่อยก่อนรันครบทุก scenario

---

## Master’s level

ควรเรียนครบทุกบทและเน้น:

- DEM-resolution implications
- maximum vs other resampling approaches
- area weighting
- radar beam-height uncertainty
- terrain horizon
- D10/D50
- MUE
- remapping uncertainty
- common-domain design
- sensitivity of site ranking
- reproducibility metadata
- distinction between screening and engineering validation

---

# How to use each notebook before pressing “Run all”

สำหรับทุก Notebook ให้นิสิตใช้ลำดับการเรียนเดียวกัน:

```text
1. Read WHY
2. Read THEORY
3. Read EQUATIONS
4. Check INPUTS
5. Inspect PARAMETERS
6. Predict the expected result
7. Run code
8. Inspect figures/tables
9. Explain physical meaning
10. State limitations
```

ไม่แนะนำให้เริ่มจาก `Runtime → Run all` โดยไม่อ่าน Markdown

---

# Quick start

1. Sign in to a Google account.
2. เปิด Notebook 00 ด้วยปุ่ม **Open in Colab**
3. ให้สิทธิ์ mount Google Drive เมื่อถูกถาม
4. ดาวน์โหลด SRTM teaching archive จาก Zenodo
5. ตรวจ readiness report ของ Notebook 00
6. รัน Notebook 01 เพื่อสร้าง 1-km UTM DEM
7. รัน Notebook 02–07 ตามลำดับ
8. ตรวจ CSV/NPZ/GeoTIFF/figures ก่อนย้ายไปบทถัดไป
9. ถ้า readiness check ไม่ผ่าน ให้แก้ปัญหาก่อน ไม่ควรข้าม
10. เก็บ metadata และ tables พร้อม figures เสมอ

---

# Typical Google Drive workspace

```text
Teaching_Radar_BeamBlockage_NaN_Thailand/
├── 00_source_data/
│   └── dem/
│       ├── srtm_56_08.tif
│       ├── srtm_56_09.tif
│       ├── srtm_56_10.tif
│       ├── srtm_57_08.tif
│       ├── srtm_57_09.tif
│       └── srtm_57_10.tif
├── 01_processed_data/
│   └── dem/
├── 02_metadata/
├── 03_results/
├── 04_figures/
└── 05_tables/
```

โครงสร้างจริงอาจมี source-sidecar files เช่น `.tfw` และ `.hdr` เพิ่มเติม

---

# Reproducibility

แบบเรียนถูกออกแบบให้รักษา processing provenance ผ่าน:

- source-data DOI
- source tile inventory
- raster metadata
- CRS metadata
- site coordinates
- native site elevation
- DEM-processing metadata
- radar geometry parameters
- beamwidth
- elevation-angle list
- range resolution
- k-factor assumption
- thresholds
- output tables
- output arrays
- figure names
- readiness checks

ไม่ควรเก็บเฉพาะ PNG สุดท้าย เพราะไม่เพียงพอสำหรับการตรวจสอบว่ารูปถูกสร้างจาก assumption ใด

---

# Scientific interpretation rules

ก่อนนำผลไปใช้ในรายงาน วิทยานิพนธ์ หรือบทความ ควรแยกคำต่อไปนี้อย่างชัดเจน

| Term | Meaning |
|---|---|
| **DEM elevation** | ค่าความสูงที่ raster แทนภูมิประเทศ |
| **Site ground elevation** | ความสูงพื้นดิน ณ radar site |
| **Radar feed altitude** | site elevation + antenna height |
| **Beam centre** | แนวกึ่งกลางของ radar beam |
| **Beam envelope** | ช่วงหน้าตัดของ beam ตาม beamwidth ที่กำหนด |
| **PBB** | สัดส่วน beam cross-section ที่ถูก terrain บัง ณ gate |
| **CBB** | cumulative/running blockage ตาม ray |
| **D10** | first range ที่ CBB ≥ 0.10 |
| **D50** | first range ที่ CBB ≥ 0.50 |
| **MUE** | elevation ต่ำสุดในชุดที่ CBB ผ่าน clear threshold |
| **Common domain** | พื้นที่ ground support ที่ใช้ร่วมกันในการเปรียบเทียบ |
| **Decision support** | หลักฐานเพื่อช่วยคัดกรอง ไม่ใช่ final engineering approval |

ตัวอย่างการเขียนผลอย่างถูกต้อง:

```text
CBB = 0.65
→ ภายใต้ DEM และ radar-geometry assumptions ที่ใช้
   cumulative beam-blockage fraction ของ ray นี้เท่ากับประมาณ 0.65

D50 = 42 km
→ CBB ของ ray นี้ถึงเกณฑ์ 0.50 ครั้งแรกที่ประมาณ 42 km

MUE = 1.5°
→ ในชุด elevation ที่ทดสอบ
   1.5° เป็นมุมยกต่ำสุดที่ CBB < 0.10 ที่ตำแหน่งนั้น

ΔCBB = −0.40
→ ณ ground cell เดียวกัน
   Phu Phiang มี modeled terrain blockage ต่ำกว่า current site 0.40
```

ข้อความเหล่านี้ **ไม่เท่ากับ**

```text
CBB = 0.65
→ radar ใช้งานไม่ได้ 65%

MUE = 1.5°
→ ต้อง operationally scan ที่ 1.5° เท่านั้น

Phu Phiang has lower CBB
→ Phu Phiang is automatically the final construction site
```

---

# Important limitations

## 1. DEM resolution

SRTM source มีรายละเอียดประมาณ 90 m แต่หลายขั้นใช้ 1-km maximum-elevation representation

ดังนั้นผลเหมาะสำหรับ:

```text
regional terrain screening
comparative teaching analysis
radar-site relative comparison
```

มากกว่า:

```text
final tower placement
building-scale obstruction
engineering line-of-sight certification
```

---

## 2. Vertical uncertainty

SRTM elevation, surveyed ground elevation และ radar feed altitude ไม่ใช่ค่าเดียวกัน

ก่อนใช้ในการออกแบบจริงควรมี:

```text
surveyed site elevation
actual antenna pedestal height
radome/feed geometry
higher-resolution local DEM
```

---

## 3. Atmospheric refraction

แบบเรียนใช้

$$
k=\frac{4}{3}
$$

ซึ่งเป็น standard-refraction assumption

แต่ refractivity จริงสามารถเปลี่ยนตาม atmospheric structure

ดังนั้น beam path จริงอาจต่างจาก model โดยเฉพาะกรณี anomalous propagation

---

## 4. Beam model

beam ถูกแทนด้วย idealized geometry และ nominal beamwidth

ระบบ radar จริงมี antenna pattern, side lobes และ system-specific characteristics ที่ละเอียดกว่านี้

---

## 5. CBB threshold

ค่า 0.10 และ 0.50 ใช้เพื่อการสอนและ comparative screening

ไม่ได้เป็น universal operational thresholds สำหรับ radar ทุกระบบ

---

## 6. Site-selection scope

การเลือก radar site จริงยังต้องพิจารณา:

```text
land ownership
road access
electric power
telecommunications
construction feasibility
maintenance access
electromagnetic interference
network overlap
hydrometeorological mission
environmental constraints
budget
```

ซึ่งไม่ได้อยู่ใน core course นี้

---

# Outputs

ตลอดหลักสูตร ผู้เรียนจะได้ทำงานกับหลายรูปแบบ:

```text
CSV      → metadata / decision tables / summary statistics
JSON     → processing provenance / parameters
NPZ      → reusable numerical arrays
GeoTIFF  → GIS-ready DEM / raster products
PNG      → scientific figures and maps
```

figure outputs เน้น:

- English scientific labels
- consistent colormaps
- physical units
- 300 dpi หรือสูงกว่า
- no unnecessary decoration
- readable station labels
- equal spatial aspect on projected maps

---

# Suggested student exercises and mini-projects

เมื่อเรียนถึง Notebook 07 สามารถต่อยอดได้ เช่น:

1. **Elevation-angle sensitivity**  
   เปรียบเทียบ CBB ที่ 0.5°, 1.0°, 1.5° และ 2.0°

2. **Directional terrain analysis**  
   เลือก azimuth ที่ blockage สูงและอธิบาย ridge ที่เป็นสาเหตุ

3. **D10/D50 mapping**  
   สร้าง polar map ของ first-blockage distance

4. **DEM-resampling sensitivity**  
   เปรียบเทียบ maximum, bilinear และ average DEM โดยอภิปรายผลเชิงภูมิประเทศ

5. **Radar tower-height experiment**  
   เปลี่ยน antenna height แล้วดูว่าทิศใดได้ประโยชน์มากที่สุด

6. **Minimum Usable Elevation**  
   เปรียบเทียบ MUE distribution ระหว่าง current และ candidate

7. **Range-band analysis**  
   วิเคราะห์ clear coverage ใน 0–50, 50–100, 100–150, 150–200 และ 200–240 km

8. **Same-ground-cell comparison**  
   สร้าง $\Delta CBB$ ระหว่าง candidate กับ current site

9. **Visibility transition**  
   แยกพื้นที่ gain/loss จากการย้าย radar

10. **Administrative extension**  
    เพิ่มขอบเขตจังหวัด/อำเภอและสรุป terrain visibility ตามพื้นที่ปกครอง

11. **Watershed extension**  
    เพิ่มลุ่มน้ำเพื่อประเมินว่า site ใดเหมาะกับ radar-rainfall monitoring ของ basin ที่กำหนด

12. **Publication figure redesign**  
    เลือก 3–4 figures จาก course แล้วปรับ layout สำหรับ manuscript

---

# Suggested assessment questions

ผู้สอนสามารถใช้คำถามต่อไปนี้เป็น quiz หรือ oral examination:

1. ทำไม radar beam สูงขึ้นตาม range แม้ elevation angle คงที่?
2. ความแตกต่างระหว่าง PBB กับ CBB คืออะไร?
3. ทำไมภูเขาที่ 30 km สามารถมีผลต่อ CBB ที่ 200 km?
4. ทำไมการนับจำนวน blocked gates จึงไม่เท่ากับ blocked area?
5. ทำไม 1-km MAX DEM อาจ overestimate near-site terrain?
6. ทำไม site บนภูเขาสูงอาจมี beam blockage แย่กว่าพื้นที่ต่ำกว่า?
7. การเพิ่ม elevation angle ช่วย terrain clearance แต่สร้างข้อเสียอะไร?
8. D10 และ D50 บอกอะไรที่ maximum CBB บอกไม่ได้?
9. MUE มีความหมายอย่างไร?
10. เหตุใด polar arrays ของ radar คนละ site จึงลบกันตรง ๆ ไม่ได้?
11. common spatial domain สร้างขึ้นเพื่ออะไร?
12. เหตุใด common-domain boundary จึงไม่จำเป็นต้องเป็นวงกลม?
13. $\Delta CBB<0$ ใน Notebook 07 หมายถึงอะไร?
14. ทำไม lower CBB ยังไม่เพียงพอที่จะรับรอง final radar site?
15. metadata ใดบ้างที่ควรรายงานเพื่อให้ beam-blockage analysis reproducible?

---

# Recommended interpretation hierarchy

สำหรับการอภิปรายผล ควรเดินตามลำดับ:

```text
1. Terrain setting
2. Radar-site elevation
3. Beam geometry
4. PBB
5. CBB
6. Directional pattern
7. Range dependence
8. Elevation dependence
9. MUE
10. Same-ground-cell comparison
11. Decision-support interpretation
12. Limitations
```

ไม่ควรกระโดดจาก DEM map ไปสู่ “site นี้ดีที่สุด” โดยไม่ผ่าน physical and spatial diagnostics

---

# Data and software acknowledgment

แบบเรียนนี้ใช้ข้อมูล SRTM DEM และแนวคิดจาก open scientific software ecosystem

Primary tools:

- wradlib
- Rasterio
- PyProj
- NumPy
- Pandas
- SciPy
- Matplotlib

SRTM source information is provided by the U.S. Geological Survey Earth Resources Observation and Science (EROS) Center and NASA Shuttle Radar Topography Mission program.

Teaching DEM archive:

**DOI: 10.5281/zenodo.22687421**

เมื่อใช้ dataset หรือ repository นี้ในงานวิจัย ควรอ้างอิงทั้ง:

1. original SRTM/USGS source
2. Zenodo teaching dataset DOI
3. software libraries ที่เกี่ยวข้อง
4. repository/course material ตามความเหมาะสม

---

# Citation / use in teaching

หากนำ repository นี้ไปใช้ในการเรียนการสอน การอบรม หรือพัฒนางานวิจัยต่อ สามารถอ้างอิงในรูปแบบทั่วไปได้ว่า:

```text
Teaching Radar–Terrain Beam Blockage with SRTM, wradlib & GIS
Python / Google Colab practical course for weather-radar beam-blockage
and radar-site terrain evaluation in Northern Thailand.

Repository:
https://github.com/nattaponm/Teaching_Radar_BeamBlockage_NaN_Thailand

Teaching DEM:
https://doi.org/10.5281/zenodo.22687421
```

---

# Repository

https://github.com/nattaponm/Teaching_Radar_BeamBlockage_NaN_Thailand

---

# Final message

เป้าหมายของ repository นี้ไม่ใช่เพียงให้ผู้เรียนสามารถสร้างแผนที่ CBB ได้ แต่ให้สามารถตอบอย่างมีเหตุผลทางภูมิศาสตร์และเรดาร์อุตุนิยมวิทยาว่า:

> **What terrain data were used?  
> How were those terrain data georeferenced and resampled?  
> How was the radar beam represented?  
> Where did terrain first intercept the beam?  
> How did blockage accumulate along the ray?  
> How did the result change with azimuth, range and elevation angle?  
> Were different radar sites compared over the same geographic support?  
> And what can — or cannot — be concluded from the resulting maps and statistics?**

หากผู้เรียนตอบคำถามเหล่านี้ได้ แสดงว่าไม่ได้เพียง “ใช้โค้ดเป็น” แต่เริ่มเข้าใจการวิเคราะห์ **weather radar as a spatial measurement system interacting with real terrain** ซึ่งเป็นพื้นฐานสำคัญสำหรับการต่อยอดสู่ radar QPE, radar mosaicking, severe-weather monitoring, hydrometeorological applications และงานวิจัยด้านภูมิสารสนเทศระดับสูง
