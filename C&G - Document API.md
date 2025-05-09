# Account

**Admin:**   
**ID:**   
**Password:** 

**Frontend:**   
**ID:**   
**Password:** 

# MMVN API

1. # Mega Menu

![][image1]  
URL: [https://local.mmpro.com/graphql](https://hpi.local.com/graphql)  
**Body**:

| {   categories(     filters: {       parent\_id: {in: \["2"\]}     }     pageSize:1     currentPage: 1   ) {     total\_count     items {       level       name    category\_icon\_image    category\_menu\_icon       children {         level         name     category\_icon\_image     category\_menu\_icon         children\_count         children {           level           name      category\_icon\_image      category\_menu\_icon         }       }     }   } } |
| :---- |
|  |

**Response:** 

| {   "data": {     "categories": {       "total\_count": 57,       "items": \[         {           "uid": "NDMwMA==",           "level": 2,           "name": "Thịt các loại",           "category\_icon\_image": null,          "category\_menu\_icon": null,           "path": "1/2/4300",           "children\_count": "3",           "children": \[             {               "uid": "NDY3NA==",               "level": 3,               "name": "Thịt bò",        "category\_icon\_image": "logo\_3.jpg",              "category\_menu\_icon": "image\_2024\_10\_02T03\_17\_28\_967Z.png",               "path": "1/2/4300/4674",               "children\_count": "2",               "children": \[                 {                   "uid": "NTIxNA==",                   "level": 4,                   "name": "Thịt tươi",          "category\_icon\_image": null,                  "category\_menu\_icon": null,                   "path": "1/2/4300/4674/5214"                 },                 {                   "uid": "NTIyOA==",                   "level": 4,                   "name": "Thịt đông lạnh",                  "category\_icon\_image": null,                  "category\_menu\_icon": null,                   "path": "1/2/4300/4674/5228"                 }               \]             },             {               "uid": "NDY3NQ==",               "level": 3,               "name": "Thịt heo",                "category\_icon\_image": null,                 "category\_menu\_icon": null,               "path": "1/2/4300/4675",               "children\_count": "2",               "children": \[                 {                   "uid": "NTI0Mw==",                   "level": 4,                   "name": "Thịt tươi",                   "category\_icon\_image": null,                  "category\_menu\_icon": null,                   "path": "1/2/4300/4675/5243"                 },                 {                   "uid": "NTI1OQ==",                   "level": 4,                   "name": "Thịt đông lạnh",                   "category\_icon\_image": null,                   "category\_menu\_icon": null,                   "path": "1/2/4300/4675/5259"                 }               \]             },             {               "uid": "NTE0MQ==",               "level": 3,               "name": "Các loại thịt khác",               "category\_icon\_image": null,                "category\_menu\_icon": null,               "path": "1/2/4300/5141",               "children\_count": "2",               "children": \[                 {                   "uid": "NTI5OQ==",                   "level": 4,                   "name": "Thịt đông lạnh",                   "category\_icon\_image": null,                  "category\_menu\_icon": null,                   "path": "1/2/4300/5141/5299"                 },                 {                   "uid": "NTMwMg==",                   "level": 4,                   "name": "Thịt tươi",                  "category\_icon\_image": null,                  "category\_menu\_icon": null,                   "path": "1/2/4300/5141/5302"                 }               \]             }           \]         }       \],       "page\_info": {         "current\_page": 1,         "page\_size": 1,         "total\_pages": 57       }     }   } } |
| :---- |

**Response Description:**

- **items**: Menu cấp 1   
  - **name:** Tên menu cấp 1  
  - **children:** Menu cấp 2  
    - **name:** Tên menu cấp 2  
    - **children:** menu cấp 3  
      - **name:** tên menu cấp 3

**Note\*:** Cách lấy link icon. Lấy value của 2 trường “category\_icon\_image” hoặc “category\_menu\_icon” tùy vào ảnh set. Rồi gắn với url domain/media/catalog/category/value\_image   
Ví dụ: value “category\_icon\_image” \= “logo\_3.jpg” \=\> url icom image là  
https://local.mmpro.vn/media/catalog/category/logo\_3.jpg

2. # Menu Header

![][image2]  
**URL**: [https://local.mmpro.vn/graphql/](https://local.mmpro.vn/graphql/)  
**Reference**: [https://developer.adobe.com/commerce/webapi/graphql/schema/store/queries/cms-blocks/](https://developer.adobe.com/commerce/webapi/graphql/schema/store/queries/cms-blocks/)  
**Header**: đưa vào giá trị store\_code của 1 store view thuộc vie hoặc eng để đưa ra cmsBlocks của store view đó. 

| Store : mm\_10010\_vi  Store : mm\_10010\_en |
| :---- |

* **Nếu như khách hàng chưa login**:  gọi tới cms block có identifier \= “header\_menu\_links”

**Body**:

| {     cmsBlocks(identifiers: \["header\_menu\_links"\]) {         items {             identifier,             title,             content         }     } } |
| :---- |

**Response**:

- **Store view vie**:

| {   "data": {     "cmsBlocks": {       "items": \[         {           "identifier": "header\_menu\_links",           "title": "\[VIE\] Header Menu Links (NO LOGIN)",           "content": "\<ul\>\\r\\n\<li\>\<a href=\\"https://mmpro.vn/san-pham-mm-mall/mm-mall-promotion/mm-mall-promotion-deal-soc.html\\"\>KHUYẾN MÃI\</a\>\</li\>\\r\\n\<li\>\<a href=\\"https://mmpro.vn/mm-mall-homepage\\"\>DỊCH VỤ MỚI\</a\>\</li\>\\r\\n\<li\>\<a href=\\"https://mmpro.vn/san-pham-mm-mall/mm-mall-freeship.html\\"\>FREE SHIPPING\</a\>\</li\>\\r\\n\<li\>\<a href=\\"https://mmpro.vn/our-solutions\\"\>GIẢI PHÁP\</a\>\</li\>\\r\\n\<li\>\<a title=\\"FOOD@MM\\" href=\\"https://mmvietnam.com/en/food-library/\\" target=\\"\_blank\\" rel=\\"noopener\\"\>FOOD@MM\</a\>\</li\>\\r\\n\<li\>\<a title=\\"OCOP\\" href=\\"https://mmpro.vn/eng-ocop-login/\\"\>OCOP\</a\>\</li\>\\r\\n\<li\>\<a href=\\"https://mmpro.vn/gia-tot-login\\"\>GIÁ TỐT\</a\>\</li\>\\r\\n\<li\>\<a title=\\"NEWS\\" href=\\"https://mmpro.vn/news\\"\>TIN TỨC\</a\>\</li\>\\r\\n\<li\>\<a href=\\"https://online.mmvietnam.com/\\"\>KHÁCH HÀNG CÁ NHÂN\</a\>\</li\>\\r\\n\</ul\>"         }       \]     }   } } |
| :---- |

- **Store view eng**:

| {   "data": {     "cmsBlocks": {       "items": \[         {           "identifier": "header\_menu\_links",           "title": "\[ENG\] Header Menu Links (NO LOGIN)",           "content": "\<ul\>\\r\\n\<li\>\<a href=\\"https://mmpro.vn/san-pham-mm-mall/mm-mall-promotion/mm-mall-promotion-deal-soc.html\\"\>PROMOTIONS\</a\>\</li\>\\r\\n\<li\>\<a href=\\"https://mmpro.vn/mm-mall-homepage\\"\>NEW SERVICE\</a\>\</li\>\\r\\n\<li\>\<a href=\\"https://mmpro.vn/san-pham-mm-mall/mm-mall-freeship.html\\"\>FREE SHIPPING\</a\>\</li\>\\r\\n\<li\>\<a href=\\"https://mmpro.vn/our-solutions\\"\>OUR SOLUTIONS\</a\>\</li\>\\r\\n\<li\>\<a title=\\"FOOD@MM\\" href=\\"https://mmvietnam.com/en/food-library/\\" target=\\"\_blank\\" rel=\\"noopener\\"\>FOOD@MM\</a\>\</li\>\\r\\n\<li\>\<a title=\\"OCOP\\" href=\\"https://mmpro.vn/eng-ocop-login/\\"\>OCOP\</a\>\</li\>\\r\\n\<li\>\<a href=\\"https://mmpro.vn/gia-tot-login\\"\>GIÁ TỐT\</a\>\</li\>\\r\\n\<li\>\<a title=\\"NEWS\\" href=\\"https://mmpro.vn/news\\"\>NEWS\</a\>\</li\>\\r\\n\<li\>\<a href=\\"https://online.mmvietnam.com/\\"\>INDIVIDUAL CUSTOMER\</a\>\</li\>\\r\\n\</ul\>"         }       \]     }   } } |
| :---- |

**Response description**:

- **items**: danh sách cms block  
  - **identifier**: tên xác định của cms block  
  - **title**: tiêu đề  
  - **content**: nội dung  
* **Nếu khách hàng đã login:** gọi tới cms block có identifier \= “header\_menu\_links\_login”

**Body**:

| {     cmsBlocks(identifiers: \["header\_menu\_links\_login"\]) {         items {             identifier,             title,             content         }     } } |
| :---- |

**Response**:

- **Store view vie:**

| {   "data": {     "cmsBlocks": {       "items": \[         {           "identifier": "header\_menu\_links\_login",           "title": "\[VI\]Header Menu Links  (LOGGED-IN)",           "content": "\<ul\>\\r\\n\<li\>\<a href=\\"https://mmpro.vn/san-pham-mm-mall/mm-mall-promotion/mm-mall-promotion-deal-soc.html\\"\>KHUYẾN MÃI\</a\>\</li\>\\r\\n\<li\>\<a href=\\"https://mmpro.vn/mm-mall-homepage\\"\>DỊCH VỤ MỚI\</a\>\</li\>\\r\\n\<li\>\<a href=\\"https://mmpro.vn/san-pham-mm-mall/mm-mall-freeship.html\\"\>FREE SHIPPING\</a\>\</li\>\\r\\n\<li\>\<a href=\\"https://mmpro.vn/our-solutions\\"\>GIẢI PHÁP\</a\>\</li\>\\r\\n\<li\>\<a title=\\"FOOD@MM\\" href=\\"https://mmvietnam.com/en/food-library/\\" target=\\"\_blank\\" rel=\\"noopener\\"\>FOOD@MM\</a\>\</li\>\\r\\n\<li\>\<a href=\\"https://mmpro.vn/gia-tot-login\\"\>GIÁ TỐT\</a\>\</li\>\\r\\n\<li\>\<a title=\\"NEWS\\" href=\\"https://mmpro.vn/news\\"\>TIN TỨC\</a\>\</li\>\\r\\n\</ul\>"         }       \]     }   } }  |
| :---- |

- **Store view eng:**

| {   "data": {     "cmsBlocks": {       "items": \[         {           "identifier": "header\_menu\_links\_login",           "title": "\[EN\] Header Menu Links (LOGGED-IN)",           "content": "\<ul\>\\r\\n\<li\>\<a href=\\"https://mmpro.vn/san-pham-mm-mall/mm-mall-promotion/mm-mall-promotion-deal-soc.html\\"\>PROMOTIONS\</a\>\</li\>\\r\\n\<li\>\<a href=\\"https://mmpro.vn/mm-mall-homepage\\"\>NEW SERVICE\</a\>\</li\>\\r\\n\<li\>\<a href=\\"https://mmpro.vn/san-pham-mm-mall/mm-mall-freeship.html\\"\>FREE SHIPPING\</a\>\</li\>\\r\\n\<li\>\<a href=\\"https://mmpro.vn/our-solutions\\"\>SOLUTIONS\</a\>\</li\>\\r\\n\<li\>\<a title=\\"FOOD@MM\\" href=\\"https://mmvietnam.com/en/food-library/\\" target=\\"\_blank\\" rel=\\"noopener\\"\>FOOD@MM\</a\>\</li\>\\r\\n\<li\>\<a href=\\"https://mmpro.vn/gia-tot-login\\"\>GIÁ TỐT\</a\>\</li\>\\r\\n\<li\>\<a title=\\"NEWS\\" href=\\"https://mmpro.vn/news\\"\>NEWS\</a\>\</li\>\\r\\n\</ul\>"         }       \]     }   } } |
| :---- |

- **Response description:**  
  - **items:** danh sách cms block  
    - **identifier:** tên xác định của cms block  
    - **title:** tiêu đề   
    - **content:** nội dung

3. # Menu Footer

![][image3]  
**URL**: [https://local.mmpro.vn/graphql/](https://local.mmpro.vn/graphql/)  
**Reference**: [https://developer.adobe.com/commerce/webapi/graphql/schema/store/queries/cms-blocks/](https://developer.adobe.com/commerce/webapi/graphql/schema/store/queries/cms-blocks/)  
**Header**: đưa vào giá trị store\_code của 1 store view thuộc vie hoặc eng để đưa ra cmsBlocks của store view đó. 

| Store : mm\_10010\_vi  Store : mm\_10010\_en |
| :---- |

**Body**:

| {     cmsBlocks(identifiers: \["footer\_our\_store", "footer\_company\_profile", "footer\_contact\_info", "footer\_copyright"\]) {         items {             identifier,             title,             content         }     } } |
| :---- |

**Response**: 

- **Store view eng**: 

| {   "data": {     "cmsBlocks": {       "items": \[         {           "identifier": "footer\_our\_store",           "title": "\[EN\] \[Footer\] Our store",           "content": "\<div class=\\"block mobile-collapse js-mobile-collapse\\"\>\\r\\n\<div class=\\"block-title\\"\>\<a href=\\"https://mmpro.vn/stores\\"\>\<strong\>Our Branches\</strong\>\</a\>\</div\>\\r\\n\<ul\>\\r\\n\<li class=\\"block-title\\"\>\<a href=\\"https://mmpro.vn/newspost/he-thong-mm-pro-khu-vuc-mien-bac-12.html\\"\>North\</a\>\</li\>\\r\\n\<li class=\\"block-title\\"\>\<a href=\\"https://mmpro.vn/newspost/he-thong-mm-pro-khu-vuc-mien-trung.html\\"\>Central\</a\>\</li\>\\r\\n\<li class=\\"block-title\\"\>\<a href=\\"https://mmpro.vn/newspost/he-thong-mm-pro-khu-vuc-mien-nam.html\\"\>South\</a\>\</li\>\\r\\n\</ul\>\\r\\n\</div\>"         },         {           "identifier": "footer\_company\_profile",           "title": "\[Footer\]Company Profile\_EN",           "content": "\<div class=\\"block mobile-collapse js-mobile-collapse\\"\>\\r\\n\<div class=\\"block-title\\"\>\<strong\>Company Profile\</strong\>\</div\>\\r\\n\<div class=\\"block-content\\"\>\\r\\n\<ul\>\\r\\n\<li\>\<a href=\\"/about-us\\"\>About us\</a\>\</li\>\\r\\n\<li\>\<a href=\\"https://mmvietnam.com/en/\\"\>MM Mega Market Vietnam\</a\>\</li\>\\r\\n\<li\>\<a href=\\"/our-awards\\"\>Awards\</a\>\</li\>\\r\\n\<li\>\<a href=\\"https://mmvietnam.com/tuyen-dung/\\" target=\\"blank\\"\>Career with us\</a\>\</li\>\\r\\n\<li\>\<a href=\\"/quality-control\\"\>Quality control\</a\>\</li\>\\r\\n\<li\>\<a href=\\"/privacy-policy\\"\>Privacy policy\</a\>\</li\>\\r\\n\<li\>\<a href=\\"/return-policy\\"\>Return policy\</a\>\</li\>\\r\\n\<li\>\<a href=\\"/trading-conditions\\"\>Trading conditions\</a\>\</li\>\\r\\n\<li\>\<a href=\\"https://mmvietnam.com/san-pham/\\" target=\\"blank\\"\>Structure \&amp; Business Portfolio\</a\>\</li\>\\r\\n\<li\>\<a href=\\"/shipping-policy\\"\>Shipping, delivery and payment policy\</a\>\</li\>\\r\\n\</ul\>\\r\\n\</div\>\\r\\n\</div\>"         },         {           "identifier": "footer\_contact\_info",           "title": "\[VN\] \[Footer\] Contact Info",           "content": "\<div class=\\"footer-contact\\"\>\<img src=\\"https://local.mmpro.vn/media/cms\_page/home/logo-mmpro.png\\" alt=\\"MMPro\\" width=\\"108\\" height=\\"32\\"\>\\r\\n\<p\>CÔNG TY TNHH MM MEGA MARKET (VIỆT NAM)\<br\>Hoạt động theo Giấy chứng nhận đăng ký doanh nghiệp số 0302249586 \<br\>do sở Kế hoạch và đầu tư Tp. Hồ Chí Minh cấp lần đầu ngày 20/07/2009\<br\>\<strong\>Khu B, KDT An Phú \- An Khánh, P. An Phú, TP. Thủ Đức, TP. HCM\</strong\>\</p\>\\r\\n\<a class=\\"phone\\" href=\\"tel:1800646878\\"\>1800646878\</a\> \<a class=\\"email\\" href=\\"mailto:contactus@mmvietnam.com\\"\>contactus@mmvietnam.com\</a\>\\r\\n\<ul class=\\"social-links\\"\>\\r\\n\<li class=\\"linkin\\"\>\<a title=\\"Mega Market Linked-in\\" href=\\"https://www.linkedin.com/company/mm-mega-market-vietnam/\\" target=\\"\_blank\\" rel=\\"noopener\\"\>Mega Market Linked-in\</a\>\</li\>\\r\\n\<li class=\\"instagram\\"\>\<a title=\\"Mega Market Instagram\\" href=\\"https://www.instagram.com/mmmegamarketvietnam/\\" target=\\"\_blank\\" rel=\\"noopener\\"\>Mega Market Instagram\</a\>\</li\>\\r\\n\<li class=\\"facebook\\"\>\<a title=\\"Mega Market Facebook\\" href=\\"https://www.facebook.com/MMMegaMarketVietnam\\" target=\\"\_blank\\" rel=\\"noopener\\"\>Mega Market Facebook\</a\>\</li\>\\r\\n\<li class=\\"youtube\\"\>\<a title=\\"Mega Market Channel\\" href=\\"https://www.youtube.com/c/MMMegaMarketVietnam/featured\\" target=\\"\_blank\\" rel=\\"noopener\\"\>Mega Market Channel\</a\>\</li\>\\r\\n\</ul\>\\r\\n\<a style=\\"margin-top: 20px;\\" href=\\"http://online.gov.vn/Home/WebDetails/84506\\" target=\\"\_blank\\" rel=\\"noopener\\"\>\<img src=\\"https://mmpro.vn/media/mm-logo-bocongthuong.png\\" alt=\\"\\" height=\\"90\\" width=\\"240\\"\>\</a\>\</div\>"         },         {           "identifier": "footer\_copyright",           "title": "\[Footer\] Copyright",           "content": "\<p\>MM © 2022\. All Rights Reserved.\</p\>"         }       \]     }   } }  |
| :---- |

- **Store view vie**: 

| {   "data": {     "cmsBlocks": {       "items": \[         {           "identifier": "footer\_our\_store",           "title": "\[VN\] \[Footer\]Our store",           "content": "\<div class=\\"block mobile-collapse js-mobile-collapse\\"\>\\r\\n\<div class=\\"block-title\\"\>\<a href=\\"https://mmpro.vn/stores\\"\>\<strong\>Hệ thống trung tâm MM MegaMarket VN\</strong\>\</a\>\</div\>\\r\\n\<ul\>\\r\\n\<li class=\\"block-title\\"\>\<a href=\\"https://mmpro.vn/newspost/he-thong-mm-pro-khu-vuc-mien-bac-12.html\\"\>Miền Bắc\</a\>\</li\>\\r\\n\<li class=\\"block-title\\"\>\<a href=\\"https://mmpro.vn/newspost/he-thong-mm-pro-khu-vuc-mien-trung.html\\"\>Miền Trung\</a\>\</li\>\\r\\n\<li class=\\"block-title\\"\>\<a href=\\"https://mmpro.vn/newspost/he-thong-mm-pro-khu-vuc-mien-nam.html\\"\>Miền Nam\</a\>\</li\>\\r\\n\</ul\>\\r\\n\</div\>"         },         {           "identifier": "footer\_company\_profile",           "title": "\[Footer\]Company Profile\_VN",           "content": "\<div class=\\"block mobile-collapse js-mobile-collapse\\"\>\\r\\n\<div class=\\"block-title\\"\>\<strong\>Thông tin công ty\</strong\>\</div\>\\r\\n\<div class=\\"block-content\\"\>\\r\\n\<ul\>\\r\\n\<li\>\<a href=\\"/about-us\\"\>Về chúng tôi\</a\>\</li\>\\r\\n\<li\>\<a href=\\"https://mmvietnam.com/\\"\>MM Mega Market Việt Nam\</a\>\</li\>\\r\\n\<li\>\<a href=\\"/our-awards\\"\>Giải thưởng\</a\>\</li\>\\r\\n\<li\>\<a href=\\"https://mmvietnam.com/tuyen-dung/\\" target=\\"blank\\"\>Tuyển dụng\</a\>\</li\>\\r\\n\<li\>\<a title=\\"MM Mega Market \- Hướng dẫn mua hàng\\" href=\\"https://mmpro.vn/huong\_dan\_mua\_hang\_h\\"\>Hướng dẫn mua hàng\</a\>\</li\>\\r\\n\<li\>\<a href=\\"/quality-control\\"\>Quản lý chất lượng\</a\>\</li\>\\r\\n\<li\>\<a href=\\"/dieu-kien-giao-dich-chung\\"\>Điều kiện giao dịch chung\</a\>\</li\>\\r\\n\<li\>\<a href=\\"https://mmvietnam.com/san-pham/\\" target=\\"blank\\"\>Cấu trúc \&amp; Danh mục kinh doanh\</a\>\</li\>\\r\\n\<li\>\<a href=\\"/privacy-policy\\"\>Chính sách bảo mật\</a\>\</li\>\\r\\n\<li\>\<a href=\\"/return-policy\\"\>Chính sách đổi trả hàng\</a\>\</li\>\\r\\n\<li\>\<a href=\\"/shipping-policy\\"\>Chính sách vận chuyển, giao nhận và thanh toán\</a\>\</li\>\\r\\n\</ul\>\\r\\n\</div\>\\r\\n\</div\>"         },         {           "identifier": "footer\_contact\_info",           "title": "\[VN\] \[Footer\] Contact Info",           "content": "\<div class=\\"footer-contact\\"\>\<img src=\\"https://local.mmpro.vn/media/cms\_page/home/logo-mmpro.png\\" alt=\\"MMPro\\" width=\\"108\\" height=\\"32\\"\>\\r\\n\<p\>CÔNG TY TNHH MM MEGA MARKET (VIỆT NAM)\<br\>Hoạt động theo Giấy chứng nhận đăng ký doanh nghiệp số 0302249586 \<br\>do sở Kế hoạch và đầu tư Tp. Hồ Chí Minh cấp lần đầu ngày 20/07/2009\<br\>\<strong\>Khu B, KDT An Phú \- An Khánh, P. An Phú, TP. Thủ Đức, TP. HCM\</strong\>\</p\>\\r\\n\<a class=\\"phone\\" href=\\"tel:1800646878\\"\>1800646878\</a\> \<a class=\\"email\\" href=\\"mailto:contactus@mmvietnam.com\\"\>contactus@mmvietnam.com\</a\>\\r\\n\<ul class=\\"social-links\\"\>\\r\\n\<li class=\\"linkin\\"\>\<a title=\\"Mega Market Linked-in\\" href=\\"https://www.linkedin.com/company/mm-mega-market-vietnam/\\" target=\\"\_blank\\" rel=\\"noopener\\"\>Mega Market Linked-in\</a\>\</li\>\\r\\n\<li class=\\"instagram\\"\>\<a title=\\"Mega Market Instagram\\" href=\\"https://www.instagram.com/mmmegamarketvietnam/\\" target=\\"\_blank\\" rel=\\"noopener\\"\>Mega Market Instagram\</a\>\</li\>\\r\\n\<li class=\\"facebook\\"\>\<a title=\\"Mega Market Facebook\\" href=\\"https://www.facebook.com/MMMegaMarketVietnam\\" target=\\"\_blank\\" rel=\\"noopener\\"\>Mega Market Facebook\</a\>\</li\>\\r\\n\<li class=\\"youtube\\"\>\<a title=\\"Mega Market Channel\\" href=\\"https://www.youtube.com/c/MMMegaMarketVietnam/featured\\" target=\\"\_blank\\" rel=\\"noopener\\"\>Mega Market Channel\</a\>\</li\>\\r\\n\</ul\>\\r\\n\<a style=\\"margin-top: 20px;\\" href=\\"http://online.gov.vn/Home/WebDetails/84506\\" target=\\"\_blank\\" rel=\\"noopener\\"\>\<img src=\\"https://mmpro.vn/media/mm-logo-bocongthuong.png\\" alt=\\"\\" height=\\"90\\" width=\\"240\\"\>\</a\>\</div\>"         },         {           "identifier": "footer\_copyright",           "title": "\[Footer\] Copyright",           "content": "\<p\>MM © 2022\. All Rights Reserved.\</p\>"         }       \]     }   } }  |
| :---- |

**Response description**:

- **items**: danh sách cms block footer  
- **identifier**: tên xác định của block  
- **title**: tiêu đề của block  
- **content**: nội dung của block

4. # Homepage

   # 

5. # Category

   ## 5.1  Suggestion Category For Guest và Suggestion Category For Customer

**URL**: [https://local.mmpro.vn/graphql/](https://local.mmpro.vn/graphql/)  
**Body**:

|   category(id: $id) {    uid    suggestion\_category {      for\_customer {        id        uid        name        url\_image        url\_key      }      for\_guest {        id        uid        name        url\_image        url\_key      }    }  } |
| :---- |

**Response:**

| {  "data": {    "category": {      "uid": "MjQ4NDg=",      "suggestion\_category": {        "for\_customer": \[          {            "id": 4298,            "uid": "NDI5OA==",            "name": "Hải sản",            "url\_image": "https://b2c.mmpro.local/media/catalog/category/pepe-the-frog-fc0xi68rw21ini90.jpg",            "url\_key": "hai-san"          },          {            "id": 4299,            "uid": "NDI5OQ==",            "name": "Rau củ \- Trái cây",            "url\_image": "",            "url\_key": "rau-cu-trai-cay"          },          {            "id": 4300,            "uid": "NDMwMA==",            "name": "Thịt các loại",            "url\_image": "",            "url\_key": "thit"          }        \],        "for\_guest": \[          {            "id": 5130,            "uid": "NTEzMA==",            "name": "Giải Pháp Văn Phòng Phẩm",            "url\_image": "",            "url\_key": "giai-phap-van-phong-pham"          }        \]      },      "\_\_typename": "CategoryTree"    }  }} |
| :---- |

## 5.2  Lấy banner category

Banner sẽ lấy tại trường **description**  
**URL**: [https://local.mmpro.vn/graphql/](https://local.mmpro.vn/graphql/)  
**Body**:

| {  categories(    filters: {      parent\_id: {in: \["2"\]}    }    pageSize:1    currentPage: 1  ) {    total\_count    items {      uid      path      level      name      category\_icon\_image      category\_menu\_icon      description      children {        level        name        category\_icon\_image        category\_menu\_icon        description        children\_count        children {          level          name          category\_icon\_image          category\_menu\_icon          description        }      }    }  } } |
| :---- |

**Response:**

| {     "data": {         "categories": {             "total\_count": 57,             "items": \[                 {                     "uid": "NDMwMA==",                     "path": "1/2/4300",                     "level": 2,                     "name": "Thịt các loại",                     "category\_icon\_image": "/media/catalog/category/thit.png",                     "category\_menu\_icon": null,                     "description": null,                     "children": \[                         {                             "level": 3,                             "name": "Thịt bò",                             "category\_icon\_image": "/media/catalog/category/logo\_3.jpg",                             "category\_menu\_icon": "/media/catalog/category/image\_2024\_10\_02T03\_17\_28\_967Z.png",                             "description": "\<style\>\#html-body \[data-pb-style=CU3MQAO\],\#html-body \[data-pb-style=XQ4AAUH\]{background-position:left top;background-size:cover;background-repeat:no-repeat;background-attachment:scroll}\#html-body \[data-pb-style=XQ4AAUH\]{justify-content:flex-start;display:flex;flex-direction:column}\#html-body \[data-pb-style=CU3MQAO\]{min-height:300px}\#html-body \[data-pb-style=F2EIDEK\]{background-color:transparent}\</style\>\<div data-content-type=\\"row\\" data-appearance=\\"contained\\" data-element=\\"main\\"\>\<div data-enable-parallax=\\"0\\" data-parallax-speed=\\"0.5\\" data-background-images=\\"{}\\" data-background-type=\\"image\\" data-video-loop=\\"true\\" data-video-play-only-visible=\\"true\\" data-video-lazy-load=\\"true\\" data-video-fallback-src=\\"\\" data-element=\\"inner\\" data-pb-style=\\"XQ4AAUH\\"\>\<div data-content-type=\\"banner\\" data-appearance=\\"collage-centered\\" data-show-button=\\"never\\" data-show-overlay=\\"never\\" data-element=\\"main\\"\>\<div data-element=\\"empty\_link\\"\>\<div class=\\"pagebuilder-banner-wrapper\\" data-background-images=\\"{\\\\\&quot;desktop\_image\\\\\&quot;:\\\\\&quot;https://local.mmpro.vn/media/wysiwyg/banner-magenest.png\\\\\&quot;,\\\\\&quot;mobile\_image\\\\\&quot;:\\\\\&quot;https://local.mmpro.vn/media/wysiwyg/logo.jpg\\\\\&quot;}\\" data-background-type=\\"image\\" data-video-loop=\\"true\\" data-video-play-only-visible=\\"true\\" data-video-lazy-load=\\"true\\" data-video-fallback-src=\\"\\" data-element=\\"wrapper\\" data-pb-style=\\"CU3MQAO\\"\>\<div class=\\"pagebuilder-overlay\\" data-overlay-color=\\"\\" aria-label=\\"\\" title=\\"\\" data-element=\\"overlay\\" data-pb-style=\\"F2EIDEK\\"\>\<div class=\\"pagebuilder-collage-content\\"\>\<div data-element=\\"content\\"\>\</div\>\</div\>\</div\>\</div\>\</div\>\</div\>\</div\>\</div\>",                             "children\_count": "2",                             "children": \[                                 {                                     "level": 4,                                     "name": "Thịt tươi",                                     "category\_icon\_image": null,                                     "category\_menu\_icon": null,                                     "description": null                                 },                                 {                                     "level": 4,                                     "name": "Thịt đông lạnh",                                     "category\_icon\_image": null,                                     "category\_menu\_icon": null,                                     "description": null                                 }                             \]                         },                         {                             "level": 3,                             "name": "Thịt heo",                             "category\_icon\_image": null,                             "category\_menu\_icon": null,                             "description": null,                             "children\_count": "2",                             "children": \[                                 {                                     "level": 4,                                     "name": "Thịt tươi",                                     "category\_icon\_image": null,                                     "category\_menu\_icon": null,                                     "description": null                                 },                                 {                                     "level": 4,                                     "name": "Thịt đông lạnh",                                     "category\_icon\_image": null,                                     "category\_menu\_icon": null,                                     "description": null                                 }                             \]                         },                         {                             "level": 3,                             "name": "Các loại thịt khác",                             "category\_icon\_image": null,                             "category\_menu\_icon": null,                             "description": null,                             "children\_count": "2",                             "children": \[                                 {                                     "level": 4,                                     "name": "Thịt đông lạnh",                                     "category\_icon\_image": null,                                     "category\_menu\_icon": null,                                     "description": null                                 },                                 {                                     "level": 4,                                     "name": "Thịt tươi",                                     "category\_icon\_image": null,                                     "category\_menu\_icon": null,                                     "description": null                                 }                             \]                         }                     \]                 }             \]         }     } } |
| :---- |

## 

6. # API Chọn store theo địa chỉ

**URL**: [https://local.mmpro.vn/graphql/](https://local.mmpro.vn/graphql/)  
**Body**:

| {   storeView(     street: "Xóm 1, Lĩnh Nam, Hoàng Mai, Hà Nội",     city: "1",     district: "8",     ward: "328",     language: "vi",     website: "b2c"   ) {     store\_view\_code {       distance       distance\_text       priority       store\_view\_code       source\_name     }     message     allow\_selection   } }  |
| :---- |

**Response:**

| {   "data": {     "storeView": {       "store\_view\_code": \[         {           "distance": 2,           "distance\_text": "2 km",           "priority": 1,           "store\_view\_code": "b2c\_10014\_vi"         },         {           "distance": 5,           "distance\_text": "5 km",           "priority": 2,           "store\_view\_code": "b2c\_10015\_vi"         }       \],       "message": null,       "allow\_selection": true,       “more\_than\_two\_sources”: true     }   } }  |
| :---- |

**Param Description:**  
      **\-** 	address: địa chỉ street ở popup hoặc street checkout  
      **\-**  	city: \= mã city\_code tỉnh/thành phố

- district: mã district\_code quận/huyện  
- ward: mã ward\_code phường/xã  
- language: ngôn ngữ Tiếng Việt (vi), tiếng Anh (en)  
- website: truyền vào “b2c” hoặc “b2b”


Response:  
allow\_selection: Cho phép hiển thị popup ( nếu danh sách store lớn hơn hoặc bằng 2 )  
more\_than\_two\_sources: Có nhiều hơn 2 source ( nếu danh sách store lớn hơn 2 )

7. # API Xác nhận vị trí người dùng

**URL**: [https://local.mmpro.vn/graphql/](https://local.mmpro.vn/graphql/)  
**Body**:

| {    locationUser (        lat: "20.9721198",        long: "105.837918",        language: "vi",        website: "b2c"    )    {          region\_id        city        city\_code        district        district\_code        ward        ward\_code        address        store\_view\_code    } } |
| :---- |

**Response:**

| {     "data": {         "locationUser": {             "region\_id": "724",             "city": "Thành phố Hà Nội",             "city\_code": "1",             "district": "Quận Hoàng Mai",             "district\_code": "8",             "ward": "Phường Đại Kim",             "ward\_code": "316",             "address": "9 Đại Từ, Đại Kim, Hoàng Mai, Hà Nội",             "store\_view\_code": "mm\_10026\_vi"         }     } } |
| :---- |

**Param Description:**  
      **\-**  	lat: Vĩ độ địa chỉ người dùng

- long: Kinh độ địa chỉ người dùng  
- language: ngôn ngữ truyền vào “vi” hoặc “en”  
- website: truyền vào “b2c” hoặc “b2b”

**Response Description:**

- region\_id: id tỉnh/thành phố  
- city: tên tỉnh/ thành phố  
- city\_code: code tỉnh/thành phố  
- district: tên quận/huyện  
- district\_code: code quận/huyện  
- ward: tên phường/xã  
- ward\_code: code phường/xã  
- address: địa chỉ  
- store\_view\_code: store view code

8. # API get config có thể tái sử dụng

**URL:** [https://local.mmpro.vn/graphql/](https://local.mmpro.vn/graphql/)  
**Header:**

| Store : mm\_10010\_vi |
| :---- |

**Body:**

| {   storeConfig {     locale     base\_currency\_code     default\_display\_currency\_code     timezone     weight\_unit     welcome     front     cms\_home\_page     no\_route     cms\_no\_cookies     show\_cms\_breadcrumbs     product\_url\_suffix     category\_url\_suffix     title\_separator     list\_mode     grid\_per\_page\_values     list\_per\_page\_values     grid\_per\_page     list\_per\_page     catalog\_default\_sort\_by     autocomplete\_on\_storefront     minimum\_password\_length     required\_character\_classes\_number     create\_account\_confirmation     customer\_access\_token\_lifetime     category\_fixed\_product\_tax\_display\_setting     product\_fixed\_product\_tax\_display\_setting     sales\_fixed\_product\_tax\_display\_setting   } } |
| :---- |

**Response:**

| {   "data": {     "storeConfig": {       "locale": "vi\_VN",       "base\_currency\_code": "VND",       "default\_display\_currency\_code": "VND",       "timezone": "Asia/Ho\_Chi\_Minh",       "weight\_unit": "kgs",       "welcome": "Welcome to Mega Market Vietnam",       "front": "cms",       "cms\_home\_page": "home",       "no\_route": "cms/noroute/index",       "cms\_no\_cookies": "enable-cookies",       "show\_cms\_breadcrumbs": 1,       "product\_url\_suffix": ".html",       "category\_url\_suffix": ".html",       "title\_separator": "-",       "list\_mode": "grid-list",       "grid\_per\_page\_values": "12,24,36",       "list\_per\_page\_values": "5,10,12,15,20,24,25",       "grid\_per\_page": 12,       "list\_per\_page": 12,       "catalog\_default\_sort\_by": "position",       "autocomplete\_on\_storefront": false,       "minimum\_password\_length": "4",       "required\_character\_classes\_number": "1",       "create\_account\_confirmation": false,       "customer\_access\_token\_lifetime": 720,       "category\_fixed\_product\_tax\_display\_setting": "FPT\_DISABLED",       "product\_fixed\_product\_tax\_display\_setting": "FPT\_DISABLED",       "sales\_fixed\_product\_tax\_display\_setting": "FPT\_DISABLED"     }   } } |
| :---- |

**Response description:**

- locale: vị trí  
- base\_currency\_code: mã tiền tệ mặc định  
- default\_display\_currency\_code: mã tiền tệ hiển thị mặc định  
- timezone: múi giờ  
- weight\_unit: đơn vị đo khối lượng  
- welcome: welcome message  
- front: which content pages using  
- cms\_home\_page: uri home page  
- no\_route: uri cms no route  
- cms\_no\_cookies: uri cms no cookie  
- show\_cms\_breadcrumbs: đường dẫn tới cms page hiển thị theo dạng Home \> About us  
- product\_url\_suffix: hậu tố url cho sản phẩm  
- category\_url\_suffix: hậu tố url cho category  
- title\_seperator: ký tự phân tách giữa tên trang và tiêu đề trang  
- list\_mode: chế độ hiển thị sản phẩm  
- grid\_per\_page\_values: xác định số lượng sản phẩm hiển thị trên mỗi trang khi ở chế độ grid  
- list\_per\_page\_values: xác định số lượng sản phẩm hiển thị trên mỗi trang khi ở chế độ list  
- grid\_per\_page: số lượng sản phẩm hiển thị mặc định trên mỗi trang ở chế độ grid  
- list\_per\_page:  số lượng sản phẩm hiển thị mặc định trên mỗi trang ở chế độ list  
- catalog\_default\_sort\_by: tiêu chí sắp xếp mặc định cho sản phẩm trong danh mục  
- autocomplete\_on\_storefront: xác định tính năng tự động hoàn thành biểu mẫu hoặc password  
- minimum\_password\_length: độ dài tối thiểu của mật khẩu  
- required\_character\_classes\_number: xác định nhóm ký tự khác nhau yêu cầu trong mật khẩu  
- create\_account\_confirmation: xác nhận qua email khi tạo tài khoản  
- customer\_access\_token\_lifetime: lifetime token đăng nhập của khách hàng  
- category\_fixed\_product\_tax\_display\_setting: xác định cách hiển thị thuế sản phẩm cố định cho danh mục  
- product\_fixed\_product\_tax\_display\_setting: xác định cách hiển thị thuế sản phẩm cố định cho sản phẩm  
- sales\_fixed\_product\_tax\_display\_setting: xác định cách hiển thị thuế sản phẩm cố định trong quá trình bán hàng

9. # API Cấu hình thời gian giao hàng

**URL**: [https://local.mmpro.vn/graphql/](https://local.mmpro.vn/graphql/)  
**Body**:

| {  storeConfig {    delivery\_time\_within    same\_day\_delivery\_time    next\_day\_delivery\_time  } } |
| :---- |

**Response:**

| {     "data": {         "storeConfig": {             "delivery\_time\_within": "4 giờ",             "same\_day\_delivery\_time": "8:00 \- 16:00",             "next\_day\_delivery\_time": "16:00"         }     } } |
| :---- |

**Response Description:**  
      **\-**  	delivery\_time\_within: Thời gian giao hàng

- same\_day\_delivery\_time: Khoảng thời gian giao hàng trong ngày  
- next\_day\_delivery\_time: Sau thời gian này thì sẽ giao hàng vào ngày hôm sau

10. # Api danh sách tìm kiếm gần đây

URL: [https://local.mmpro.vn/graphql/](https://local.mmpro.vn/graphql/)  
Body:

| {   popularSearchKeywords(pageSize: 10, currentPage: 1) {     total\_count     items {       keyword       count     }     page\_info {       page\_size       current\_page       total\_pages     }   } } |
| :---- |

Response:

| {   "data": {     "popularSearchKeywords": {       "total\_count": 406723,       "items": \[         {           "keyword": "RUOU TRẮNG",           "count": 31332         },         {           "keyword": "Xanh la",           "count": 7032         },         {           "keyword": "banh",           "count": 2772         },         {           "keyword": "compound trắng",           "count": 2502         },         {           "keyword": "TUI RAC",           "count": 2219         },         {           "keyword": "dau an",           "count": 2146         },         {           "keyword": "nước rửa chén",           "count": 1929         },         {           "keyword": "táo",           "count": 1893         },         {           "keyword": "Kẹo",           "count": 1869         },         {           "keyword": "Trứng gà",           "count": 1826         }       \],       "page\_info": {         "page\_size": 10,         "current\_page": 1,         "total\_pages": 40673       }     }   } } |
| :---- |

Response description:

- total\_count: số từ đã được tìm kiếm  
- items: các từ được kiếm nhiều theo độ phổ biến giảm dần  
  - keyword: từ  
  - count: độ phổ biến  
- page\_info: thông tin phân trang  
  - page\_size: số từ trong 1 trang  
  - current\_page: trang hiện tại  
  - total\_pages: tổng số trang

11. # API đăng ký tài khoản

**\- [Link Docs](https://developer.adobe.com/commerce/webapi/graphql/schema/customer/mutations/create/)**  
**\- Body:**

| mutation {   createCustomerV2(     input: {       firstname: "Minh"       lastname: "Mon"       email: "nmhlam213@gmail.com"       password: "minhmon@gmail.com"       is\_subscribed: true       custom\_attributes: \[         {           attribute\_code: "company\_user\_phone\_number"           value: "989888999"         }       \]     }   ) {     customer {       firstname       lastname       email       is\_subscribed       custom\_attributes {         code         ... on AttributeValue {           value         }         ... on AttributeSelectedOptions {           selected\_options {             label             value           }         }       }     }   } } |
| :---- |

- **Response:**

| {  "data": {     "createCustomerV2": {       "customer": {         "firstname": "Minh",         "lastname": "Mon",         "email": "nmhlam213@gmail.com",         "is\_subscribed": true,         "custom\_attributes": \[           null,           {             "code": "company\_user\_phone\_number",             "value": "989888999"           },           null,           null,           null,           null,           null,           null,           {             "code": "handovered",             "selected\_options": \[\]           },           {             "code": "active\_in\_month",             "selected\_options": \[\]           },           {             "code": "is\_active\_handovered",             "selected\_options": \[\]           },           {             "code": "is\_assigned",             "value": "0"           }         \]       }     }   } } |
| :---- |

12. # API đăng nhập

**\- [Link Docs](https://developer.adobe.com/commerce/webapi/graphql/schema/customer/queries/customer/)**  
**\- Generate Customer Token:**

+ Body

| mutation {   generateCustomerToken(     email: "phuong.lam@gmail.com"     password: "securePassword123"   ) {     token   } } |
| :---- |

+ Response:

| {   "data": {     "generateCustomerToken": {       "token": "eyJraWQiOiIxIiwiYWxnIjoiSFMyNTYifQ.eyJ1aWQiOjQsInV0eXBpZCI6MywiaWF0IjoxNzI3OTQxMDkzLCJleHAiOjE3Mjc5NDQ2OTN9.CfedUnBqCCpiQTMq4vU0yfIGl1rcN\_8gaYz8XQ3Fo7w"     }   } } |
| :---- |

- **Customer Query**

+ Body

| {   customer {     firstname     lastname     suffix     email     addresses {       firstname       lastname       street       city       region {         region\_code         region       }       postcode       country\_code       telephone     }   } } |
| :---- |

+ Response:

| {   "data": {     "customer": {       "firstname": "Phuong",       "lastname": "Lam",       "suffix": null,       "email": "phuong.lam@gmail.com",       "addresses": \[\]     }   } } |
| :---- |

- **API cập nhật Phone Number:**  
+ Body

| mutation {   updateCustomerV2(     input: {       custom\_attributes: \[         {           attribute\_code: "company\_user\_phone\_number"           value: "0989041062"         }       \]     }   ) {     customer {       firstname       custom\_attributes {         code         ... on AttributeValue {           value         }       }     }   } } |
| :---- |


+ Response:

|  "data": {     "updateCustomerV2": {       "customer": {         "custom\_attributes": \[           {             "code": "company\_user\_job\_title",             "value": "abc123"           },           {             "code": "company\_user\_phone\_number",             "value": "0989041062"           },           null,           null,           null,           null,           {             "code": "is\_email\_notify",             "value": "0"           },           null,           {             "code": "handovered"           },           {             "code": "active\_in\_month"           },           {             "code": "is\_active\_handovered"           },           {             "code": "is\_assigned",             "value": "0"           },           {             "code": "magenest\_sociallogin\_id",             "value": "114373490460657507498"           },           {             "code": "magenest\_sociallogin\_type",             "value": "google"           }         \]       }     }   } |
| :---- |

13. # API Social Login

**\- Social Login 1**

+ Body

| mutation SocialLogin(    $provider: String\!,    $token: String\! ) {    socialLogin(        input: {            provider: $provider,            token: $token        }    ) {        token        customer {            id            email            firstname            lastname        }    } }  VD: {    "provider": "google",    "token": "4/0AVG7fiRy1VevW6430nPvUP730UfVsQ2Ixvxr2snbM\_emU55k2qbRMEa49svrEmp8EYcxcQ" } |
| :---- |

+ Response:

|  {    "data": {        "socialLogin": {            "token": "",            "customer": {                "id": "113729189716874014865",                "email": "nmhung222@gmail.com",                "firstname": "Hung",                "lastname": "Minh"            }        }    } } |
| :---- |

**\- Social Login 2**

+ Body

| mutation SocialLogin(    $provider: String\!,    $user\_info: CustomerSocialLoginInfo\!,    $custom\_attributes: \[AttributeValueInput\!\] ) {    socialLogin(        input: {            provider: $provider,            user\_info: $user\_info            custom\_attributes: $custom\_attributes        }    ) {        token        customer {            email            firstname            lastname        }    } } VD: {    "provider": "google",    "user\_info": {        "id": "113729189716874014865",        "email": "nmhung222@gmail.com",             "name": "Hung Minh",        "first\_name": "Hung",        "last\_name": "Minh"    },    "custom\_attributes": \[           {            "attribute\_code": "company\_user\_phone\_number",            "value": "0989041062"        }    \] } |
| :---- |

+ Response:

| {    "data": {        "socialLogin": {            "token": "eyJraWQiOiIxIiwiYWxnIjoiSFMyNTYifQ.eyJ1aWQiOjIzNjc2LCJ1dHlwaWQiOjMsImlhdCI6MTczMDM2MDA4NywiZXhwIjoxNzMwMzYzNjg3fQ.1OFfWVFg1MkWVxYn3DzI4sONDnGMF5LdUxySsDa6T9Q",            "customer": {                "email": "nmhung222@gmail.com",                "firstname": "Hung",                "lastname": "Minh"            }        }    } } |
| :---- |

14. # API quên mật khẩu, đặt mật khẩu mới

**\- Request Password Reset Email:**

+ Body

| mutation {   requestPasswordResetEmail(     email: "phuong.lam@gmail.com"   ) } |
| :---- |

+ Response:

| {   "data": {     "requestPasswordResetEmail": true   } } |
| :---- |

- **Reset Password**

| mutation {   resetPassword(     email: "phuong.lam@gmail.com",     resetPasswordToken: "securePassword123",     newPassword: "newSecurePassword123"   ) } |
| :---- |

- Response:

| {   "data": {     "resetPassword": true   } } |
| :---- |

15. # Api danh sách reviews của product

**URL**: [https://local.mmpro.vn/graphql/](https://local.mmpro.vn/graphql/)  
**Body**:  
Note: thêm header với store b2c để hiển thị do store b2b hiện đang k sử dụng product reviews.

| query {   products (     filter: { sku: { eq: "98555ab"}}   ) {     items {       sku       name       review\_distribution {         level         count         percentage       }       review\_count       reviews {         items {           nickname           summary           text           average\_rating           created\_at           ratings\_breakdown {             name             value           }         }       }     }   } } |
| :---- |

**Param description:** 

- sku: mã sản phẩm

**Response**:

|  |
| :---- |

**Response** **description**

16. # Api thêm review cho product

**URL**: [https://local.mmpro.vn/graphql/](https://local.mmpro.vn/graphql/)  
**Reference**: [createProductReview mutation](https://developer.adobe.com/commerce/webapi/graphql/schema/products/mutations/create-review/)  
**Body**:

| mutation {   createProductReview(     input: {       sku: "422124",       nickname: "Roni",       summary: "Great looking sweatshirt",       text: "This sweatshirt looks and feels great. The zipper sometimes sticks a bit.",       ratings: \[         {           id: "MQ==",           value\_id: "NA=="         },         {           id: "Mg==",           value\_id: "OA=="         },         {           id: "Mw==",           value\_id: "MTU="         }       \]     } ) {     review {       product {         rating\_summary         review\_count       }       nickname       summary       text       average\_rating       ratings\_breakdown {         name         value       }       created\_at     }   } } |
| :---- |

**Param description:**

- sku: mã sản phẩm  
- nickname: tên người review  
- summary: tiêu đề của review  
- text: mô tả của review  
- ratings: các đánh giá  
  - id: id tương ứng với phần muốn review (value, price, quality,...)  
  - value\_id: số sao đánh giá (1-\>5)

**Các giá trị của** id**,** value\_id **được lấy theo api sau:** 

| query {   productReviewRatingsMetadata {     items {       id       name       values {         value\_id         value       }     }   } } |
| :---- |

**Response body of create review**:

| {   "data": {     "createProductReview": {       "review": {         "product": {           "rating\_summary": 80,           "review\_count": 3         },         "nickname": "Roni",         "summary": "Great looking sweatshirt",         "text": "This sweatshirt looks and feels great. The zipper sometimes sticks a bit.",         "average\_rating": 80,         "ratings\_breakdown": \[           {             "name": "Quality",             "value": "4"           },           {             "name": "Value",             "value": "3"           },           {             "name": "Price",             "value": "5"           }         \],         "created\_at": "2024-10-04 02:20:45"       }     }   } } |
| :---- |

**Response** **description**:

17. # Api xem danh sách sản phẩm trong wishlist

**URL**: [https://local.mmpro.vn/graphql/](https://local.mmpro.vn/graphql/)  
**Reference**: [wishlist query](https://developer.adobe.com/commerce/webapi/graphql/schema/wishlist/queries/wishlist/)  
**Body**:

| {   customer {     wishlist\_v2(id: 1) {       id             items\_count       sharing\_code       updated\_at       items\_v2(pageSize: 10, currentPage: 1) {         items {           id           quantity           description           added\_at           product {             sku             name             description {               html             }           }         }         page\_info {           page\_size           current\_page           total\_pages         }       }     }   } } |
| :---- |

**Param description**:

- **id**: unique Id của wishlist

**Response**:

|  |
| :---- |

**Response** **description**:

18. # Api thêm sản phẩm vào wishlist

**URL**: [https://local.mmpro.vn/graphql/](https://local.mmpro.vn/graphql/)  
**Reference**: [addProductsToWishlist mutation](https://developer.adobe.com/commerce/webapi/graphql/schema/wishlist/mutations/add-products/)  
**Body**: 

| mutation {   addProductsToWishlist(     wishlistId: 0,     wishlistItems: \[       {         sku: "442776"         quantity: 2       }     \]   ) {     wishlist {       id       items\_count       sharing\_code       updated\_at       items\_v2 {         items {           id           added\_at           description           quantity            product {             uid             sku             name             price\_range {               maximum\_price {                 regular\_price {                   currency                   value                 }                 final\_price{                   currency                   value                 }                 discount {                   percent\_off                   amount\_off                 }               }               minimum\_price {                 regular\_price {                   currency                   value                 }                 final\_price{                   currency                   value                 }                 discount {                   percent\_off                   amount\_off                 }               }             }           }         }         page\_info{           current\_page           page\_size           total\_pages         }       }     }   } } |
| :---- |

**Param** **description**:

- wishlistId: id của wishlist  
- sku: mã sản phẩm  
- quantity: số lượng sản phẩm

**Response**:

|  |
| :---- |

**Response** **description**:

19. # Api xoá sản phẩm khỏi wishlist

**URL**: [https://local.mmpro.vn/graphql/](https://local.mmpro.vn/graphql/)  
**Reference**: [removeProductsFromWishlist mutation](https://developer.adobe.com/commerce/webapi/graphql/schema/wishlist/mutations/remove-products/)  
**Body**:

| mutation {   removeProductsFromWishlist (     wishlistId: 0,     wishlistItemsIds: \[       21     \]   ) {     wishlist {       id       items\_count       sharing\_code       updated\_at       items\_v2 {         items {           id           added\_at           description           product {             uid             sku             name             meta\_description             url\_key           }         }         page\_info{           current\_page           page\_size           total\_pages         }       }     }   } } |
| :---- |

**Param** **description**:

- wishlistId: id của wishlist  
- wishlistItemsIds: một mảng chứa id của (các) sản phẩm muốn gỡ bỏ

**Response**:

|  |
| :---- |

**Response** **description**:

20. # API Directory

    ## 19.1  Lấy danh sách tỉnh/thành phố

**URL**: https://local.mmpro.vn/graphql?query={cities(country\_id: "VN") {id name city\_code}}   
**Method: GET**  
**Param Description:**  
      **\-**  	country\_id: Truyền vào \= VN để lấy danh sách tỉnh Việt Nam  
**Response:**

| {     "data": {         "cities": \[             {                 "id": 702,                 "name": "Thành phố Cần Thơ",                 "city\_code": "92"             },             {                 "id": 724,                 "name": "Thành phố Hà Nội",                 "city\_code": "1"             },             {                 "id": 735,                 "name": "Thành phố Hải Phòng",                 "city\_code": "31"             },             {                 "id": 746,                 "name": "Thành phố Hồ Chí Minh",                 "city\_code": "79"             },             {                 "id": 713,                 "name": "Thành phố Đà Nẵng",                 "city\_code": "48"             },             {                 "id": 757,                 "name": "Tỉnh An Giang",                 "city\_code": "89"             },             {                 "id": 762,                 "name": "Tỉnh Bà Rịa \- Vũng Tàu",                 "city\_code": "77"             },             {                 "id": 763,                 "name": "Tỉnh Bắc Giang",                 "city\_code": "24"             },             {                 "id": 764,                 "name": "Tỉnh Bắc Kạn",                 "city\_code": "6"             },             {                 "id": 703,                 "name": "Tỉnh Bạc Liêu",                 "city\_code": "95"             },             {                 "id": 704,                 "name": "Tỉnh Bắc Ninh",                 "city\_code": "27"             },             {                 "id": 705,                 "name": "Tỉnh Bến Tre",                 "city\_code": "83"             },             {                 "id": 707,                 "name": "Tỉnh Bình Dương",                 "city\_code": "74"             },             {                 "id": 708,                 "name": "Tỉnh Bình Phước",                 "city\_code": "70"             },             {                 "id": 709,                 "name": "Tỉnh Bình Thuận",                 "city\_code": "60"             },             {                 "id": 706,                 "name": "Tỉnh Bình Định",                 "city\_code": "52"             },             {                 "id": 710,                 "name": "Tỉnh Cà Mau",                 "city\_code": "96"             },             {                 "id": 711,                 "name": "Tỉnh Cao Bằng",                 "city\_code": "4"             },             {                 "id": 718,                 "name": "Tỉnh Gia Lai",                 "city\_code": "64"             },             {                 "id": 719,                 "name": "Tỉnh Hà Giang",                 "city\_code": "2"             },             {                 "id": 720,                 "name": "Tỉnh Hà Nam",                 "city\_code": "35"             },             {                 "id": 721,                 "name": "Tỉnh Hà Tĩnh",                 "city\_code": "42"             },             {                 "id": 722,                 "name": "Tỉnh Hải Dương",                 "city\_code": "30"             },             {                 "id": 723,                 "name": "Tỉnh Hậu Giang",                 "city\_code": "93"             },             {                 "id": 725,                 "name": "Tỉnh Hoà Bình",                 "city\_code": "17"             },             {                 "id": 726,                 "name": "Tỉnh Hưng Yên",                 "city\_code": "33"             },             {                 "id": 727,                 "name": "Tỉnh Khánh Hòa",                 "city\_code": "56"             },             {                 "id": 728,                 "name": "Tỉnh Kiên Giang",                 "city\_code": "91"             },             {                 "id": 729,                 "name": "Tỉnh Kon Tum",                 "city\_code": "62"             },             {                 "id": 730,                 "name": "Tỉnh Lai Châu",                 "city\_code": "12"             },             {                 "id": 731,                 "name": "Tỉnh Lâm Đồng",                 "city\_code": "68"             },             {                 "id": 732,                 "name": "Tỉnh Lạng Sơn",                 "city\_code": "20"             },             {                 "id": 733,                 "name": "Tỉnh Lào Cai",                 "city\_code": "10"             },             {                 "id": 734,                 "name": "Tỉnh Long An",                 "city\_code": "80"             },             {                 "id": 736,                 "name": "Tỉnh Nam Định",                 "city\_code": "36"             },             {                 "id": 737,                 "name": "Tỉnh Nghệ An",                 "city\_code": "40"             },             {                 "id": 738,                 "name": "Tỉnh Ninh Bình",                 "city\_code": "37"             },             {                 "id": 739,                 "name": "Tỉnh Ninh Thuận",                 "city\_code": "58"             },             {                 "id": 740,                 "name": "Tỉnh Phú Thọ",                 "city\_code": "25"             },             {                 "id": 741,                 "name": "Tỉnh Phú Yên",                 "city\_code": "54"             },             {                 "id": 742,                 "name": "Tỉnh Quảng Bình",                 "city\_code": "44"             },             {                 "id": 743,                 "name": "Tỉnh Quảng Nam",                 "city\_code": "49"             },             {                 "id": 744,                 "name": "Tỉnh Quảng Ngãi",                 "city\_code": "51"             },             {                 "id": 745,                 "name": "Tỉnh Quảng Ninh",                 "city\_code": "22"             },             {                 "id": 747,                 "name": "Tỉnh Quảng Trị",                 "city\_code": "45"             },             {                 "id": 748,                 "name": "Tỉnh Sóc Trăng",                 "city\_code": "94"             },             {                 "id": 749,                 "name": "Tỉnh Sơn La",                 "city\_code": "14"             },             {                 "id": 750,                 "name": "Tỉnh Tây Ninh",                 "city\_code": "72"             },             {                 "id": 751,                 "name": "Tỉnh Thái Bình",                 "city\_code": "34"             },             {                 "id": 752,                 "name": "Tỉnh Thái Nguyên",                 "city\_code": "19"             },             {                 "id": 753,                 "name": "Tỉnh Thanh Hóa",                 "city\_code": "38"             },             {                 "id": 754,                 "name": "Tỉnh Thừa Thiên Huế",                 "city\_code": "46"             },             {                 "id": 755,                 "name": "Tỉnh Tiền Giang",                 "city\_code": "82"             },             {                 "id": 756,                 "name": "Tỉnh Trà Vinh",                 "city\_code": "84"             },             {                 "id": 758,                 "name": "Tỉnh Tuyên Quang",                 "city\_code": "8"             },             {                 "id": 759,                 "name": "Tỉnh Vĩnh Long",                 "city\_code": "86"             },             {                 "id": 760,                 "name": "Tỉnh Vĩnh Phúc",                 "city\_code": "26"             },             {                 "id": 761,                 "name": "Tỉnh Yên Bái",                 "city\_code": "15"             },             {                 "id": 712,                 "name": "Tỉnh Đắk Lắk",                 "city\_code": "66"             },             {                 "id": 714,                 "name": "Tỉnh Đắk Nông",                 "city\_code": "67"             },             {                 "id": 715,                 "name": "Tỉnh Điện Biên",                 "city\_code": "11"             },             {                 "id": 716,                 "name": "Tỉnh Đồng Nai",                 "city\_code": "75"             },             {                 "id": 717,                 "name": "Tỉnh Đồng Tháp",                 "city\_code": "87"             }         \]     } } |
| :---- |

## 19.2  Lấy danh sách quận/ huyện

**URL**: https://local.mmpro.vn/graphql?query={districts(city\_code: "1") {id name district\_code}}  
**Method: GET**  
**Param Description:**  
      **\-**  	city\_code: lấy từ api get danh sách tỉnh/ thành phố  
**Response:**

| {     "data": {         "districts": \[             {                 "id": 730,                 "name": "Quận Ba Đình",                 "district\_code": "1"             },             {                 "id": 731,                 "name": "Quận Hoàn Kiếm",                 "district\_code": "2"             },             {                 "id": 732,                 "name": "Quận Tây Hồ",                 "district\_code": "3"             },             {                 "id": 733,                 "name": "Quận Long Biên",                 "district\_code": "4"             },             {                 "id": 734,                 "name": "Quận Cầu Giấy",                 "district\_code": "5"             },             {                 "id": 735,                 "name": "Quận Đống Đa",                 "district\_code": "6"             },             {                 "id": 736,                 "name": "Quận Hai Bà Trưng",                 "district\_code": "7"             },             {                 "id": 737,                 "name": "Quận Hoàng Mai",                 "district\_code": "8"             },             {                 "id": 738,                 "name": "Quận Thanh Xuân",                 "district\_code": "9"             },             {                 "id": 739,                 "name": "Huyện Sóc Sơn",                 "district\_code": "16"             },             {                 "id": 740,                 "name": "Huyện Đông Anh",                 "district\_code": "17"             },             {                 "id": 741,                 "name": "Huyện Gia Lâm",                 "district\_code": "18"             },             {                 "id": 742,                 "name": "Quận Nam Từ Liêm",                 "district\_code": "19"             },             {                 "id": 755,                 "name": "Huyện Thanh Trì",                 "district\_code": "20"             },             {                 "id": 756,                 "name": "Quận Bắc Từ Liêm",                 "district\_code": "21"             },             {                 "id": 889,                 "name": "Huyện Mê Linh",                 "district\_code": "250"             },             {                 "id": 901,                 "name": "Quận Hà Đông",                 "district\_code": "268"             },             {                 "id": 902,                 "name": "Thị xã Sơn Tây",                 "district\_code": "269"             },             {                 "id": 903,                 "name": "Huyện Ba Vì",                 "district\_code": "271"             },             {                 "id": 904,                 "name": "Huyện Phúc Thọ",                 "district\_code": "272"             },             {                 "id": 905,                 "name": "Huyện Đan Phượng",                 "district\_code": "273"             },             {                 "id": 906,                 "name": "Huyện Hoài Đức",                 "district\_code": "274"             },             {                 "id": 907,                 "name": "Huyện Quốc Oai",                 "district\_code": "275"             },             {                 "id": 908,                 "name": "Huyện Thạch Thất",                 "district\_code": "276"             },             {                 "id": 909,                 "name": "Huyện Chương Mỹ",                 "district\_code": "277"             },             {                 "id": 910,                 "name": "Huyện Thanh Oai",                 "district\_code": "278"             },             {                 "id": 911,                 "name": "Huyện Thường Tín",                 "district\_code": "279"             },             {                 "id": 912,                 "name": "Huyện Phú Xuyên",                 "district\_code": "280"             },             {                 "id": 913,                 "name": "Huyện Ứng Hòa",                 "district\_code": "281"             },             {                 "id": 914,                 "name": "Huyện Mỹ Đức",                 "district\_code": "282"             }         \]     } } |
| :---- |

## 19.3  Lấy danh sách phường/xã

**URL**: https://local.mmpro.vn/graphql?query={wards(district\_code: "2") {id name ward\_code}}  
**Method: GET**  
**Param Description:**  
      **\-**  	district\_code: lấy từ api get danh sách quận/huyện  
**Response:**

| {     "data": {         "wards": \[             {                 "id": 212,                 "name": "Phường Trần Hưng Đạo",                 "ward\_code": "82"             },             {                 "id": 213,                 "name": "Phường Hàng Bông",                 "ward\_code": "76"             },             {                 "id": 214,                 "name": "Phường Cửa Nam",                 "ward\_code": "73"             },             {                 "id": 215,                 "name": "Phường Hàng Trống",                 "ward\_code": "70"             },             {                 "id": 216,                 "name": "Phường Chương Dương Độ",                 "ward\_code": "67"             },             {                 "id": 217,                 "name": "Phường Hàng Gai",                 "ward\_code": "64"             },             {                 "id": 218,                 "name": "Phường Hàng Bạc",                 "ward\_code": "61"             },             {                 "id": 219,                 "name": "Phường Lý Thái Tổ",                 "ward\_code": "58"             },             {                 "id": 220,                 "name": "Phường Cửa Đông",                 "ward\_code": "55"             },             {                 "id": 221,                 "name": "Phường Hàng Buồm",                 "ward\_code": "46"             },             {                 "id": 222,                 "name": "Phường Hàng Mã",                 "ward\_code": "43"             },             {                 "id": 223,                 "name": "Phường Đồng Xuân",                 "ward\_code": "40"             },             {                 "id": 224,                 "name": "Phường Phúc Tân",                 "ward\_code": "37"             },             {                 "id": 225,                 "name": "Phường Phan Chu Trinh",                 "ward\_code": "85"             },             {                 "id": 226,                 "name": "Phường Hàng Bài",                 "ward\_code": "88"             },             {                 "id": 8360,                 "name": "Phường Tràng Tiền",                 "ward\_code": "79"             },             {                 "id": 8361,                 "name": "Phường Hàng Đào",                 "ward\_code": "49"             },             {                 "id": 9575,                 "name": "Phường Hàng Bồ",                 "ward\_code": "52"             },             {                 "id": 11175,                 "name": "Phường Chương Dương",                 "ward\_code": "68"             }         \]     } } |
| :---- |

21. # API Search textbox

    ## 20.1 Get CMS Block Content

**URL**: [https://local.mmpro.vn/graphql/](https://local.mmpro.vn/graphql/)  
**Body**:

| {  getCmsBlockContent {    categoryBlock    customBlock  } }  |
| :---- |

**Response:**

| {    "data": {        "getCmsBlockContent": {            "categoryBlock": "\<p style=\\"text-align: center;\\"\>\<a href=\\"https://mmpro.vn/san-pham-mm-mall/mm-mall-promotion/mm-mall-promotion-deal-soc.html\\"\>\<strong\>Xem thêm\</strong\>\</a\>\</p\>\\r\\n\<p style=\\"text-align: center;\\"\>\&nbsp;\</p\>\\r\\n\<p style=\\"text-align: center;\\"\>\&nbsp;\</p\>",            "customBlock": "\<div data-content-type=\\"html\\" data-appearance=\\"default\\" data-element=\\"main\\"\>\&lt;div\&gt;abc\&lt;/div\&gt;\</div\>"        }    } } } |
| :---- |

## 

## 20.2 Get Search Suggestions

**URL**: [https://local.mmpro.vn/graphql/](https://local.mmpro.vn/graphql/)  
**Body**:

| {  getSearchSuggestions(q: "nguoi nhen", asm\_uid: "1828080401")  {    suggestions {        type        id        form\_key        qty        offer\_id        min\_order\_quantity        max\_order\_quantity        package\_quantity        quantity        action        is\_dnr        sku        art\_no        image        title        url        price        ecom\_name        mm\_barcode        breadcrumb        count    } } }      |
| :---- |

**Response:**

| {    "data": {        "getSearchSuggestions": {            "suggestions": \[                {                    "type": "term",                    "id": null,                    "form\_key": null,                    "qty": null,                    "offer\_id": null,                    "min\_order\_quantity": null,                    "max\_order\_quantity": null,                    "package\_quantity": null,                    "quantity": null,                    "action": null,                    "is\_dnr": null,                    "sku": null,                    "art\_no": null,                    "image": null,                    "title": "áo mưa",                    "url": null,                    "price": null,                    "ecom\_name": null,                    "mm\_barcode": null,                    "breadcrumb": null,                    "count": null                },                {                    "type": "term",                    "id": null,                    "form\_key": null,                    "qty": null,                    "offer\_id": null,                    "min\_order\_quantity": null,                    "max\_order\_quantity": null,                    "package\_quantity": null,                    "quantity": null,                    "action": null,                    "is\_dnr": null,                    "sku": null,                    "art\_no": null,                    "image": null,                    "title": "áo mưa nylon",                    "url": null,                    "price": null,                    "ecom\_name": null,                    "mm\_barcode": null,                    "breadcrumb": null,                    "count": null                },                {                    "type": "term",                    "id": null,                    "form\_key": null,                    "qty": null,                    "offer\_id": null,                    "min\_order\_quantity": null,                    "max\_order\_quantity": null,                    "package\_quantity": null,                    "quantity": null,                    "action": null,                    "is\_dnr": null,                    "sku": null,                    "art\_no": null,                    "image": null,                    "title": "áo mưa bộ",                    "url": null,                    "price": null,                    "ecom\_name": null,                    "mm\_barcode": null,                    "breadcrumb": null,                    "count": null                },                {                    "type": "term",                    "id": null,                    "form\_key": null,                    "qty": null,                    "offer\_id": null,                    "min\_order\_quantity": null,                    "max\_order\_quantity": null,                    "package\_quantity": null,                    "quantity": null,                    "action": null,                    "is\_dnr": null,                    "sku": null,                    "art\_no": null,                    "image": null,                    "title": "áo mưa rạng đông",                    "url": null,                    "price": null,                    "ecom\_name": null,                    "mm\_barcode": null,                    "breadcrumb": null,                    "count": null                },                {                    "type": "term",                    "id": null,                    "form\_key": null,                    "qty": null,                    "offer\_id": null,                    "min\_order\_quantity": null,                    "max\_order\_quantity": null,                    "package\_quantity": null,                    "quantity": null,                    "action": null,                    "is\_dnr": null,                    "sku": null,                    "art\_no": null,                    "image": null,                    "title": "ÁO MƯA DÙNG 1 LẦN",                    "url": null,                    "price": null,                    "ecom\_name": null,                    "mm\_barcode": null,                    "breadcrumb": null,                    "count": null                },                {                    "type": "term",                    "id": null,                    "form\_key": null,                    "qty": null,                    "offer\_id": null,                    "min\_order\_quantity": null,                    "max\_order\_quantity": null,                    "package\_quantity": null,                    "quantity": null,                    "action": null,                    "is\_dnr": null,                    "sku": null,                    "art\_no": null,                    "image": null,                    "title": "ÁO MƯA GIẤY",                    "url": null,                    "price": null,                    "ecom\_name": null,                    "mm\_barcode": null,                    "breadcrumb": null,                    "count": null                },                {                    "type": "product",                    "id": "350023",                    "form\_key": "z7xZ2uayYjhru4Ez",                    "qty": 1,                    "offer\_id": null,                    "min\_order\_quantity": null,                    "max\_order\_quantity": null,                    "package\_quantity": null,                    "quantity": null,                    "action": "http://mmpro.local/checkout/cart/add/uenc/aHR0cDovL21tcHJvLmxvY2FsL2dyYXBocWw\_WERFQlVHX1NFU1NJT05fU1RBUlQ9UEhQU1RPUk0\~/product/350023/",                    "is\_dnr": false,                    "sku": null,                    "art\_no": null,                    "image": "http://mmpro.local/media/catalog/product/placeholder/default/logommvn-180.jpg",                    "title": "AO MUA QV CD TRON",                    "url": "http://mmpro.local/qv-rain-coat-plain-350023.html",                    "price": "\<div class=\\"price-box price-final\_price\\" data-role=\\"priceBox\\" data-product-id=\\"350023\\" data-price-box=\\"product-id-350023\\"\>\\n            \\n\\n\<span class=\\"price-container price-final\_price&\#x20;tax&\#x20;weee\\"\\n        \>\\n        \<span  id=\\"product-price-350023\\"                data-price-amount=\\"153704\\"\\n        data-price-type=\\"finalPrice\\"\\n        class=\\"price-wrapper \\"\\n    \>\<span class=\\"price\\"\>153.704  VNĐ\</span\>\</span\>\\n        \</span\>\\n    \\n    \<span class=\\"offer-wrapper\\" style=\\"display: none;\\"\>\\n                    \<div class=\\"offer-min-shipping-price offer-shipping best-offer-shipping\\" data-shipping-offer-id=\\"\\" style=\\"display: none\\"\>\\n                \+ \<span class=\\"offer-min-shipping-price-amount-excl-tax\\"\>\\n                    0  VNĐ                \</span\>\&nbsp;shipping            \</div\>\\n            \</span\>\\n\\n\\n\<div class=\\"offer-price-description\\" style=\\"display: none;\\"\>\\n    \</div\>\\n\</div\>",                    "ecom\_name": "Áo mưa QV CD tròn",                    "mm\_barcode": "24148733",                    "breadcrumb": null,                    "count": null                },                {                    "type": "product",                    "id": "350020",                    "form\_key": "z7xZ2uayYjhru4Ez",                    "qty": 1,                    "offer\_id": null,                    "min\_order\_quantity": null,                    "max\_order\_quantity": null,                    "package\_quantity": null,                    "quantity": null,                    "action": "http://mmpro.local/checkout/cart/add/uenc/aHR0cDovL21tcHJvLmxvY2FsL2dyYXBocWw\_WERFQlVHX1NFU1NJT05fU1RBUlQ9UEhQU1RPUk0\~/product/350020/",                    "is\_dnr": false,                    "sku": null,                    "art\_no": null,                    "image": "http://mmpro.local/media/catalog/product/placeholder/default/logommvn-180.jpg",                    "title": "AO MUA QV TRON KM",                    "url": "http://mmpro.local/qv-rain-coat-plain-km-350020.html",                    "price": "\<div class=\\"price-box price-final\_price\\" data-role=\\"priceBox\\" data-product-id=\\"350020\\" data-price-box=\\"product-id-350020\\"\>\\n            \\n\\n\<span class=\\"price-container price-final\_price&\#x20;tax&\#x20;weee\\"\\n        \>\\n        \<span  id=\\"product-price-350020\\"                data-price-amount=\\"63889\\"\\n        data-price-type=\\"finalPrice\\"\\n        class=\\"price-wrapper \\"\\n    \>\<span class=\\"price\\"\>63.889  VNĐ\</span\>\</span\>\\n        \</span\>\\n    \\n    \<span class=\\"offer-wrapper\\" style=\\"display: none;\\"\>\\n                    \<div class=\\"offer-min-shipping-price offer-shipping best-offer-shipping\\" data-shipping-offer-id=\\"\\" style=\\"display: none\\"\>\\n                \+ \<span class=\\"offer-min-shipping-price-amount-excl-tax\\"\>\\n                    0  VNĐ                \</span\>\&nbsp;shipping            \</div\>\\n            \</span\>\\n\\n\\n\<div class=\\"offer-price-description\\" style=\\"display: none;\\"\>\\n    \</div\>\\n\</div\>",                    "ecom\_name": "Áo mưa QV trơn KM",                    "mm\_barcode": "24148696",                    "breadcrumb": null,                    "count": null                },                {                    "type": "product",                    "id": "347985",                    "form\_key": "z7xZ2uayYjhru4Ez",                    "qty": 1,                    "offer\_id": null,                    "min\_order\_quantity": null,                    "max\_order\_quantity": null,                    "package\_quantity": null,                    "quantity": null,                    "action": "http://mmpro.local/checkout/cart/add/uenc/aHR0cDovL21tcHJvLmxvY2FsL2dyYXBocWw\_WERFQlVHX1NFU1NJT05fU1RBUlQ9UEhQU1RPUk0\~/product/347985/",                    "is\_dnr": false,                    "sku": null,                    "art\_no": null,                    "image": "http://mmpro.local/media/catalog/product/placeholder/default/logommvn-180.jpg",                    "title": "AO MUA TIEN LOI AKICO AK",                    "url": "http://mmpro.local/raincoat-akico-ak-347985.html",                    "price": "\<div class=\\"price-box price-final\_price\\" data-role=\\"priceBox\\" data-product-id=\\"347985\\" data-price-box=\\"product-id-347985\\"\>\\n            \\n\\n\<span class=\\"price-container price-final\_price&\#x20;tax&\#x20;weee\\"\\n        \>\\n        \<span  id=\\"product-price-347985\\"                data-price-amount=\\"14722\\"\\n        data-price-type=\\"finalPrice\\"\\n        class=\\"price-wrapper \\"\\n    \>\<span class=\\"price\\"\>14.722  VNĐ\</span\>\</span\>\\n        \</span\>\\n    \\n    \<span class=\\"offer-wrapper\\" style=\\"display: none;\\"\>\\n                    \<div class=\\"offer-min-shipping-price offer-shipping best-offer-shipping\\" data-shipping-offer-id=\\"\\" style=\\"display: none\\"\>\\n                \+ \<span class=\\"offer-min-shipping-price-amount-excl-tax\\"\>\\n                    0  VNĐ                \</span\>\&nbsp;shipping            \</div\>\\n            \</span\>\\n\\n\\n\<div class=\\"offer-price-description\\" style=\\"display: none;\\"\>\\n    \</div\>\\n\</div\>",                    "ecom\_name": "Áo mưa Tiến Lợi Akico AK",                    "mm\_barcode": "24034180",                    "breadcrumb": null,                    "count": null                },                {                    "type": "product",                    "id": "350021",                    "form\_key": "z7xZ2uayYjhru4Ez",                    "qty": 1,                    "offer\_id": null,                    "min\_order\_quantity": null,                    "max\_order\_quantity": null,                    "package\_quantity": null,                    "quantity": null,                    "action": "http://mmpro.local/checkout/cart/add/uenc/aHR0cDovL21tcHJvLmxvY2FsL2dyYXBocWw\_WERFQlVHX1NFU1NJT05fU1RBUlQ9UEhQU1RPUk0\~/product/350021/",                    "is\_dnr": false,                    "sku": null,                    "art\_no": null,                    "image": "http://mmpro.local/media/catalog/product/placeholder/default/logommvn-180.jpg",                    "title": "AO MUA QV HOA TIET KM",                    "url": "http://mmpro.local/qv-rain-coat-km-350021.html",                    "price": "\<div class=\\"price-box price-final\_price\\" data-role=\\"priceBox\\" data-product-id=\\"350021\\" data-price-box=\\"product-id-350021\\"\>\\n            \\n\\n\<span class=\\"price-container price-final\_price&\#x20;tax&\#x20;weee\\"\\n        \>\\n        \<span  id=\\"product-price-350021\\"                data-price-amount=\\"95370\\"\\n        data-price-type=\\"finalPrice\\"\\n        class=\\"price-wrapper \\"\\n    \>\<span class=\\"price\\"\>95.370  VNĐ\</span\>\</span\>\\n        \</span\>\\n    \\n    \<span class=\\"offer-wrapper\\" style=\\"display: none;\\"\>\\n                    \<div class=\\"offer-min-shipping-price offer-shipping best-offer-shipping\\" data-shipping-offer-id=\\"\\" style=\\"display: none\\"\>\\n                \+ \<span class=\\"offer-min-shipping-price-amount-excl-tax\\"\>\\n                    0  VNĐ                \</span\>\&nbsp;shipping            \</div\>\\n            \</span\>\\n\\n\\n\<div class=\\"offer-price-description\\" style=\\"display: none;\\"\>\\n    \</div\>\\n\</div\>",                    "ecom\_name": "Áo mưa QV hoa tiết KM",                    "mm\_barcode": "24148702",                    "breadcrumb": null,                    "count": null                },                {                    "type": "product",                    "id": "370169",                    "form\_key": "z7xZ2uayYjhru4Ez",                    "qty": 1,                    "offer\_id": "8252",                    "min\_order\_quantity": "0",                    "max\_order\_quantity": "0",                    "package\_quantity": "0",                    "quantity": "20",                    "action": "http://mmpro.local/checkout/cart/add/uenc/aHR0cDovL21tcHJvLmxvY2FsL2dyYXBocWw\_WERFQlVHX1NFU1NJT05fU1RBUlQ9UEhQU1RPUk0\~/product/370169/",                    "is\_dnr": false,                    "sku": null,                    "art\_no": null,                    "image": "http://mmpro.local/media/catalog/product/placeholder/default/logommvn-180.jpg",                    "title": "AM 1 LAN",                    "url": "http://mmpro.local/am-1-lan-437343-24373432-20240604.html",                    "price": "\<div class=\\"price-box price-final\_price\\" data-role=\\"priceBox\\" data-product-id=\\"370169\\" data-price-box=\\"product-id-370169\\"\>\\n            \\n\\n\<span class=\\"price-container price-final\_price&\#x20;tax&\#x20;weee\\"\\n        \>\\n        \<span  id=\\"product-price-370169\\"                data-price-amount=\\"1\\"\\n        data-price-type=\\"finalPrice\\"\\n        class=\\"price-wrapper \\"\\n    \>\<span class=\\"price\\"\>1  VNĐ\</span\>\</span\>\\n        \</span\>\\n    \\n    \<span class=\\"offer-wrapper\\" style=\\"display: none;\\"\>\\n                    \<div class=\\"offer-min-shipping-price offer-shipping best-offer-shipping\\" data-shipping-offer-id=\\"\\" style=\\"display: none\\"\>\\n                \+ \<span class=\\"offer-min-shipping-price-amount-excl-tax\\"\>\\n                    0  VNĐ                \</span\>\&nbsp;shipping            \</div\>\\n            \</span\>\\n\\n\\n\<div class=\\"offer-price-description\\" style=\\"display: none;\\"\>\\n    \</div\>\\n\</div\>",                    "ecom\_name": "Áo mưa 1 lần",                    "mm\_barcode": "24373432",                    "breadcrumb": null,                    "count": null                },                {                    "type": "product",                    "id": "350022",                    "form\_key": "z7xZ2uayYjhru4Ez",                    "qty": 1,                    "offer\_id": null,                    "min\_order\_quantity": null,                    "max\_order\_quantity": null,                    "package\_quantity": null,                    "quantity": null,                    "action": "http://mmpro.local/checkout/cart/add/uenc/aHR0cDovL21tcHJvLmxvY2FsL2dyYXBocWw\_WERFQlVHX1NFU1NJT05fU1RBUlQ9UEhQU1RPUk0\~/product/350022/",                    "is\_dnr": false,                    "sku": null,                    "art\_no": null,                    "image": "http://mmpro.local/media/catalog/product/placeholder/default/logommvn-180.jpg",                    "title": "AO MUA QV CD HOA TIET",                    "url": "http://mmpro.local/qv-rain-coat-stripe-350022.html",                    "price": "\<div class=\\"price-box price-final\_price\\" data-role=\\"priceBox\\" data-product-id=\\"350022\\" data-price-box=\\"product-id-350022\\"\>\\n            \\n\\n\<span class=\\"price-container price-final\_price&\#x20;tax&\#x20;weee\\"\\n        \>\\n        \<span  id=\\"product-price-350022\\"                data-price-amount=\\"153704\\"\\n        data-price-type=\\"finalPrice\\"\\n        class=\\"price-wrapper \\"\\n    \>\<span class=\\"price\\"\>153.704  VNĐ\</span\>\</span\>\\n        \</span\>\\n    \\n    \<span class=\\"offer-wrapper\\" style=\\"display: none;\\"\>\\n                    \<div class=\\"offer-min-shipping-price offer-shipping best-offer-shipping\\" data-shipping-offer-id=\\"\\" style=\\"display: none\\"\>\\n                \+ \<span class=\\"offer-min-shipping-price-amount-excl-tax\\"\>\\n                    0  VNĐ                \</span\>\&nbsp;shipping            \</div\>\\n            \</span\>\\n\\n\\n\<div class=\\"offer-price-description\\" style=\\"display: none;\\"\>\\n    \</div\>\\n\</div\>",                    "ecom\_name": "Áo mưa QV cánh đôi họa tiết",                    "mm\_barcode": "24148719",                    "breadcrumb": null,                    "count": null                },                {                    "type": "product",                    "id": "101105",                    "form\_key": "z7xZ2uayYjhru4Ez",                    "qty": 1,                    "offer\_id": null,                    "min\_order\_quantity": null,                    "max\_order\_quantity": null,                    "package\_quantity": null,                    "quantity": null,                    "action": "http://mmpro.local/checkout/cart/add/uenc/aHR0cDovL21tcHJvLmxvY2FsL2dyYXBocWw\_WERFQlVHX1NFU1NJT05fU1RBUlQ9UEhQU1RPUk0\~/product/101105/",                    "is\_dnr": false,                    "sku": null,                    "art\_no": null,                    "image": "http://mmpro.local/media/catalog/product/placeholder/default/logommvn-180.jpg",                    "title": "AO MUA KIENG DEN QUA TANG MM",                    "url": "http://mmpro.local/gift-raincoat-mm-2017-101105.html",                    "price": "\<div class=\\"price-box price-final\_price\\" data-role=\\"priceBox\\" data-product-id=\\"101105\\" data-price-box=\\"product-id-101105\\"\>\\n            \\n\\n\<span class=\\"price-container price-final\_price&\#x20;tax&\#x20;weee\\"\\n        \>\\n        \<span  id=\\"product-price-101105\\"                data-price-amount=\\"46296\\"\\n        data-price-type=\\"finalPrice\\"\\n        class=\\"price-wrapper \\"\\n    \>\<span class=\\"price\\"\>46.296  VNĐ\</span\>\</span\>\\n        \</span\>\\n    \\n    \<span class=\\"offer-wrapper\\" style=\\"display: none;\\"\>\\n                    \<div class=\\"offer-min-shipping-price offer-shipping best-offer-shipping\\" data-shipping-offer-id=\\"\\" style=\\"display: none\\"\>\\n                \+ \<span class=\\"offer-min-shipping-price-amount-excl-tax\\"\>\\n                    0  VNĐ                \</span\>\&nbsp;shipping            \</div\>\\n            \</span\>\\n\\n\\n\<div class=\\"offer-price-description\\" style=\\"display: none;\\"\>\\n    \</div\>\\n\</div\>",                    "ecom\_name": "Áo mưa kiếng đèn quà tặng MM",                    "mm\_barcode": "22521965",                    "breadcrumb": null,                    "count": null                },                {                    "type": "category",                    "id": null,                    "form\_key": null,                    "qty": null,                    "offer\_id": null,                    "min\_order\_quantity": null,                    "max\_order\_quantity": null,                    "package\_quantity": null,                    "quantity": null,                    "action": null,                    "is\_dnr": null,                    "sku": null,                    "art\_no": null,                    "image": null,                    "title": "Móc áo",                    "url": "http://mmpro.local/trang-tri-nha-c-a/d-ng-c-gi-t-i/moc-ao.html",                    "price": null,                    "ecom\_name": null,                    "mm\_barcode": null,                    "breadcrumb": \[                        "Hàng gia dụng",                        "Dụng cụ giặt ủi"                    \],                    "count": null                },                {                    "type": "category",                    "id": null,                    "form\_key": null,                    "qty": null,                    "offer\_id": null,                    "min\_order\_quantity": null,                    "max\_order\_quantity": null,                    "package\_quantity": null,                    "quantity": null,                    "action": null,                    "is\_dnr": null,                    "sku": null,                    "art\_no": null,                    "image": null,                    "title": "Áo choàng tắm",                    "url": "http://mmpro.local/trang-tri-nha-c-a/d-dung-nha-t-m/khan-bong/ao-choang-t-m.html",                    "price": null,                    "ecom\_name": null,                    "mm\_barcode": null,                    "breadcrumb": \[                        "Hàng gia dụng",                        "Đồ dùng nhà tắm",                        "Khăn bông"                    \],                    "count": null                },                {                    "type": "category",                    "id": null,                    "form\_key": null,                    "qty": null,                    "offer\_id": null,                    "min\_order\_quantity": null,                    "max\_order\_quantity": null,                    "package\_quantity": null,                    "quantity": null,                    "action": null,                    "is\_dnr": null,                    "sku": null,                    "art\_no": null,                    "image": null,                    "title": "Móc áo gỗ",                    "url": "http://mmpro.local/trang-tri-nha-c-a/d-ng-c-gi-t-i/moc-ao/moc-ao-g.html",                    "price": null,                    "ecom\_name": null,                    "mm\_barcode": null,                    "breadcrumb": \[                        "Hàng gia dụng",                        "Dụng cụ giặt ủi",                        "Móc áo"                    \],                    "count": null                },                {                    "type": "category",                    "id": null,                    "form\_key": null,                    "qty": null,                    "offer\_id": null,                    "min\_order\_quantity": null,                    "max\_order\_quantity": null,                    "package\_quantity": null,                    "quantity": null,                    "action": null,                    "is\_dnr": null,                    "sku": null,                    "art\_no": null,                    "image": null,                    "title": "Móc áo nhựa",                    "url": "http://mmpro.local/trang-tri-nha-c-a/d-ng-c-gi-t-i/moc-ao/moc-ao-nh-a.html",                    "price": null,                    "ecom\_name": null,                    "mm\_barcode": null,                    "breadcrumb": \[                        "Hàng gia dụng",                        "Dụng cụ giặt ủi",                        "Móc áo"                    \],                    "count": null                },                {                    "type": "category",                    "id": null,                    "form\_key": null,                    "qty": null,                    "offer\_id": null,                    "min\_order\_quantity": null,                    "max\_order\_quantity": null,                    "package\_quantity": null,                    "quantity": null,                    "action": null,                    "is\_dnr": null,                    "sku": null,                    "art\_no": null,                    "image": null,                    "title": "Giỏ đựng quần áo",                    "url": "http://mmpro.local/trang-tri-nha-c-a/d-ng-c-v-sinh/gi-d-ng-qu-n-ao.html",                    "price": null,                    "ecom\_name": null,                    "mm\_barcode": null,                    "breadcrumb": \[                        "Hàng gia dụng",                        "Dụng cụ vệ sinh"                    \],                    "count": null                }            \]        }    } }  |
| :---- |

## 20.3 Get Popular Keywords

**URL**: [https://local.mmpro.vn/graphql/](https://local.mmpro.vn/graphql/)  
**Body**:

| {    getPopularKeywords {        items {            name            image\_full\_path            url            url\_pwa        }        total\_count        version        history\_max    } }  |
| :---- |

**Response:**

| {    "data": {        "getPopularKeywords": {            "items": \[                {                    "name": "MM Mall",                    "image\_full\_path": "https:\\/\\/mmpro.vn\\/media\\/catalog\\/popularsearch\\/360\_F\_90200846\_rAoY6CMRSJ7X8gFJyzyXKYtvxQqK28Lk.jpg",                    "url": "https://mmpro.vn/mm-mall-homepage",                    "url\_pwa": "/mm/marketsellers?title=MM%20Mall"                },                {                    "name": "Promotion",                    "image\_full\_path": "https:\\/\\/mmpro.vn\\/media\\/catalog\\/popularsearch\\/360\_F\_90200846\_rAoY6CMRSJ7X8gFJyzyXKYtvxQqK28Lk.jpg",                    "url": "https://mmpro.vn/san-pham-mm-mall/mm-mall-promotion/mm-mall-promotion-deal-soc.html",                    "url\_pwa": "/mm/marketsellers?title=MM%20Mall"                },                {                    "name": "HCM-Fast delivery",                    "image\_full\_path": "https:\\/\\/mmpro.vn\\/media\\/catalog\\/popularsearch\\/360\_F\_90200846\_rAoY6CMRSJ7X8gFJyzyXKYtvxQqK28Lk.jpg",                    "url": "https://mmpro.vn/san-pham-mm-mall/mm-mall-hcm-giao-hang-nhanh.html",                    "url\_pwa": "/category/24656?title=HCM+Fast+Delivery"                }            \],            "total\_count": 3,            "version": 24,            "history\_max": 5        }    } }  |
| :---- |

## 20.4 Get Config autocomplete

**URL**: [https://local.mmpro.vn/graphql/](https://local.mmpro.vn/graphql/)  
**Body**:

| { storeConfig {   historyAutocomplete   popularTermAutocomplete   productAutocomplete   categoryAutocomplete } }  |
| :---- |

**Response:**

| {    "data": {        "storeConfig": {            "historyAutocomplete": "5",            "popularTermAutocomplete": "8",            "productAutocomplete": "7",            "categoryAutocomplete": "5"        }    } }  |
| :---- |

22. # 

23. # API sản phẩm chi tiết

**\- [Link Docs](https://developer.adobe.com/commerce/webapi/graphql/schema/products/queries/products/)**  
**\- Body**  
\+ sku: SKU sản phẩm  
\+ name: Tên sản phẩm  
\+ price\_range: Min, max price và thông tin discount  
\+ categories: Danh mục sản phẩm  
\+ reviews: Thông tin đánh giá sản phẩm  
\+ image/small\_image/thumbnail/media\_gallery: Ảnh và thư viện sản phẩm  
\+ related\_products: Sản phẩm Liên quan  
\+ similar\_products: Sản phẩm cùng loại  
\+ upsell\_products: Danh sách sản phẩm upsell  
\+ crosssell\_products: Danh sách sản phẩm crosssell  
\+ mm\_brand: Thương hiệu sản phẩm

| {   products(     filter: {       sku: {         eq: "WS12"       }     }   ) {     items {       sku       \_\_typename       uid       name       mm\_brand       price\_range {         minimum\_price {           regular\_price {             value             currency           }           final\_price {             value             currency           }           discount {             amount\_off             percent\_off           }         }         maximum\_price {           regular\_price {             value             currency           }           final\_price {             value             currency           }           discount {             amount\_off             percent\_off           }         }       }       categories {         uid         name         path       }       reviews {         items {           average\_rating           ratings\_breakdown {             name             value           }         }       }       short\_description {         html       }       description {         html       }       image {         url       }       small\_image {         url       }       thumbnail {         url       }       media\_gallery {         url         label         position       }       related\_products {         uid         sku         name       }      similar\_products {         uid         sku         name       },       upsell\_products {         uid         sku         name       }       crosssell\_products {         uid         sku         name       }     }   } }  |
| :---- |

- **Response**

| {   "data": {     "products": {       "items": \[         {           "sku": "WS12",           "\_\_typename": "ConfigurableProduct",           "uid": "MTU1Ng==",           "name": "Radiant Tee",           "price\_range": {             "minimum\_price": {               "regular\_price": {                 "value": 22,                 "currency": "GBP"               },               "final\_price": {                 "value": 22,                 "currency": "GBP"               },               "discount": {                 "amount\_off": 0,                 "percent\_off": 0               }             },             "maximum\_price": {               "regular\_price": {                 "value": 22,                 "currency": "GBP"               },               "final\_price": {                 "value": 22,                 "currency": "GBP"               },               "discount": {                 "amount\_off": 0,                 "percent\_off": 0               }             }           },           "categories": \[\],           "reviews": {             "items": \[               {                 "average\_rating": 60,                 "ratings\_breakdown": \[                   {                     "name": "Rating",                     "value": "3"                   }                 \]               },               {                 "average\_rating": 40,                 "ratings\_breakdown": \[                   {                     "name": "Rating",                     "value": "2"                   }                 \]               },               {                 "average\_rating": 80,                 "ratings\_breakdown": \[                   {                     "name": "Rating",                     "value": "4"                   }                 \]               }             \]           },           "short\_description": {             "html": ""           },           "description": {             "html": "\<p\>So light and comfy, you'll love the Radiant Tee's organic fabric, feel, performance and style. You may never want to stop moving in this shirt.\</p\>\\r\\n\<p\>\&bull; Salmon soft scoop neck tee.\<br /\>\&bull; Athletic, semi-form fit.\<br /\>\&bull; Flat seams prevent chafing.\<br /\>\&bull; 67% Organic Cotton / 28% Hemp / 5% Spandex.\</p\>"           },           "image": {             "url": "https://mm.local/media/catalog/product/cache/87535547ef4caa67d8eeb490e367f748/w/s/ws12-orange\_main\_2.jpg"           },           "small\_image": {             "url": "https://mm.local/media/catalog/product/cache/87535547ef4caa67d8eeb490e367f748/w/s/ws12-orange\_main\_2.jpg"           },           "thumbnail": {             "url": "https://mm.local/media/catalog/product/cache/87535547ef4caa67d8eeb490e367f748/w/s/ws12-orange\_main\_2.jpg"           },           "media\_gallery": \[             {               "url": "https://mm.local/media/catalog/product/cache/87535547ef4caa67d8eeb490e367f748/w/s/ws12-orange\_main\_2.jpg",               "label": "Radiant Tee",               "position": 1             },             {               "url": "https://mm.local/media/catalog/product/cache/87535547ef4caa67d8eeb490e367f748/w/s/ws12-orange\_back\_2.jpg",               "label": "Radiant Tee",               "position": 2             }           \],           "related\_products": \[             {               "uid": "MQ==",               "sku": "24-MB01",               "name": "Joust Duffle Bag"             },             {               "uid": "NA==",               "sku": "24-MB05",               "name": "Wayfarer Messenger Bag"             },             {               "uid": "Mw==",               "sku": "24-MB03",               "name": "Crown Summit Backpack"             },             {               "uid": "Mg==",               "sku": "24-MB04",               "name": "Strive Shoulder Pack"             },             {               "uid": "NQ==",               "sku": "24-MB06",               "name": "Rival Field Messenger"             }           \],           "upsell\_products": \[             {               "uid": "MTU3Mg==",               "sku": "WS01",               "name": "Gwyn Endurance Tee"             },             {               "uid": "MTQxMg==",               "sku": "WS02",               "name": "Gabrielle Micro Sleeve Top"             },             {               "uid": "MTQyOA==",               "sku": "WS03",               "name": "Iris Workout Top"             },             {               "uid": "MTQ0NA==",               "sku": "WS04",               "name": "Layla Tee"             },             {               "uid": "MTU4OA==",               "sku": "WS05",               "name": "Desiree Fitness Tee"             },             {               "uid": "MTQ2MA==",               "sku": "WS06",               "name": "Elisa EverCool\&trade; Tee"             },             {               "uid": "MTQ3Ng==",               "sku": "WS07",               "name": "Juliana Short-Sleeve Tee"             },             {               "uid": "MTQ5Mg==",               "sku": "WS08",               "name": "Minerva LumaTech\&trade; V-Tee"             },             {               "uid": "MTUwOA==",               "sku": "WS09",               "name": "Tiffany Fitness Tee"             },             {               "uid": "MTUyNA==",               "sku": "WS10",               "name": "Karissa V-Neck Tee"             },             {               "uid": "MTU0MA==",               "sku": "WS11",               "name": "Diva Gym Tee"             }           \],           "crosssell\_products": \[             {               "uid": "MTU=",               "sku": "24-UG06",               "name": "Affirm Water Bottle "             },             {               "uid": "MTY=",               "sku": "24-UG07",               "name": "Dual Handle Cardio Ball"             },             {               "uid": "MjE=",               "sku": "24-WG084",               "name": "Sprite Foam Yoga Brick"             }           \]         }       \]     }   } }  |
| :---- |

24. # API Lấy thông tin cửa hàng

**URL**: https://local.mmpro.vn/graphql?query={storeInformation(store\_view\_code: "mm\_10010\_vi") {source\_code name address}}  
**Method: GET**  
**Param Description:**  
      **\-**  	store\_view\_code: Truyền vào store view code  
**Response:**

| {     "data": {         "storeInformation": {             "source\_code": "10010",             "name": "MM AN PHU",             "address": "Lot B, An Phu, An Khanh, An Phu Ward"         }     } } |
| :---- |

**Response Description:**

- source\_code: mã source \= mã store  
- name: tên cửa hàng  
- address: địa chỉ cửa hàng

25. # Đặt hàng nhanh

    ## 23.1  Lấy danh sách đặt hàng nhanh

**URL**: https://local.mmpro.vn/graphql  
**Method: POST**  
**Header:**

| Authorization : token\_customer login |
| :---- |

**Body:**

| {    getQuickOrder(        customer\_id: "23667"    )    {          id        items {            item\_id            sku            product\_id            product\_name            product\_image            art\_no            qty        }    } } |
| :---- |

**Response:**

| {     "data": {         "getQuickOrder": {             "id": 255220,             "items": \[                 {                     "item\_id": 1696893,                     "sku": "228460\_22284600",                     "product\_id": 95039,                     "product\_name": "Ba chỉ bò cắt lát xuất xứ Mỹ, 500g",                     "product\_image": "https://local.mmpro.vn/static/version1728461420/frontend/Smartosc/MM/vi\_VN/Magento\_Catalog/images/product/placeholder/.jpg",                     "art\_no": "228460",                     "qty": 1                 },                 {                     "item\_id": 1696892,                     "sku": "213306\_22133069",                     "product\_id": 95038,                     "product\_name": "Ba chỉ bò cắt lát xuất xứ Mỹ, 500g",                     "product\_image": "https://local.mmpro.vn/static/version1728461420/frontend/Smartosc/MM/vi\_VN/Magento\_Catalog/images/product/placeholder/.jpg",                     "art\_no": "213306",                     "qty": 1                 },                 {                     "item\_id": 1696891,                     "sku": "253954\_22539540",                     "product\_id": 95042,                     "product\_name": "Ba chỉ bò cắt lát xuất xứ Mỹ, 300g",                     "product\_image": "https://local.mmpro.vn/static/version1728461420/frontend/Smartosc/MM/vi\_VN/Magento\_Catalog/images/product/placeholder/.jpg",                     "art\_no": "253954",                     "qty": 1                 },                 {                     "item\_id": 1696890,                     "sku": "232483\_22324832",                     "product\_id": 95040,                     "product\_name": "Ba chỉ bò cắt lát xuất xứ Mỹ, 300g",                     "product\_image": "https://local.mmpro.vn/static/version1728461420/frontend/Smartosc/MM/vi\_VN/Magento\_Catalog/images/product/placeholder/.jpg",                     "art\_no": "232483",                     "qty": 1                 }             \]         }     } }                 |
| :---- |

## 23.2  Thêm từng sản phẩm kèm qty vào quick order

**URL**: https://local.mmpro.vn/graphql  
**Method: POST**  
**Header:**

| Authorization : token\_customer login |
| :---- |

**Body:**

| mutation {  addSingleProductToListQuickOrder(    input: {      sku: "232483",      qty: 1    }  ) {    success    message    quick\_order {      id      items {        item\_id        sku        qty      }    }  } } |
| :---- |

**Response:**

| {     "data": {         "addSingleProductToListQuickOrder": {             "success": true,             "message": "Thêm BA CHI BO CAT LAT 300GR XX MY thành công vào danh sách đặt hàng nhanh",             "quick\_order": {                 "id": 255208,                 "items": \[                     {                         "item\_id": 1696859,                         "sku": "232483\_22324832",                         "qty": 1                     },                     {                         "item\_id": 1696858,                         "sku": "253954\_22539540",                         "qty": 2                     },                     {                         "item\_id": 1696857,                         "sku": "413664\_24136648",                         "qty": 20019                     }                 \]             }         }     } } |
| :---- |

## 23.3  Thêm nhiều sản phẩm vào quick order

**URL**: https://local.mmpro.vn/graphql  
**Method: POST**  
**Header:**

| Authorization : token\_customer login |
| :---- |

**Body:**

| mutation {  addMultipleProductsToListQuickOrder(    input: {      skus: "232483,253954,213306,228460"    }  ) {    success    message    quick\_order {      id      items {        item\_id        sku        qty      }    }  } } |
| :---- |

**Param Description:**

- **skus:** danh sách sku và cách nhau bởi dấu ,

**Response:**

| {     "data": {         "addMultipleProductsToListQuickOrder": {             "success": true,             "message": " Đã thêm 4 sản phẩm vào danh sách đặt hàng nhanh",             "quick\_order": {                 "id": 255208,                 "items": \[                     {                         "item\_id": 1696861,                         "sku": "213306\_22133069",                         "qty": 4                     },                     {                         "item\_id": 1696860,                         "sku": "228460\_22284600",                         "qty": 4                     },                     {                         "item\_id": 1696859,                         "sku": "232483\_22324832",                         "qty": 13                     },                     {                         "item\_id": 1696858,                         "sku": "253954\_22539540",                         "qty": 10                     },                     {                         "item\_id": 1696857,                         "sku": "413664\_24136648",                         "qty": 20019                     }                 \]             }         }     } } |
| :---- |

## 23.4  Thêm sản phẩm bằng file csv

**URL**: https://local.mmpro.vn/graphql  
**Method: POST**  
**Header:**

| Authorization : token\_customer login |
| :---- |

**Body:**

| mutation {  addProductsByFileToListQuickOrder(    input: {      base64\_encoded\_data: "QXJ0Tm8sUXVhbnRpdHkKMjI4NDYwLDIKMjEzMzA2LDE="    }  ) {    success    message    quick\_order {      id      items {        item\_id        sku        qty      }    }  } } |
| :---- |

**Param Description:**

- **base64\_encoded\_data:** base64 code của file csv. File csv gồm 2 cột tên là ArtNo và Quantity. Có thể tại file mẫu tại Trang Đặt hàng nhanh của MMPRO

Có thể tham khảo:  
[https://www.atatus.com/tools/csv-to-base64](https://www.atatus.com/tools/csv-to-base64)   
**Response:**

| {     "data": {         "addProductsByFileToListQuickOrder": {             "success": true,             "message": " Đã thêm 2 sản phẩm vào danh sách đặt hàng nhanh",             "quick\_order": {                 "id": 255218,                 "items": \[                     {                         "item\_id": 1696895,                         "sku": "213306\_22133069",                         "qty": 4                     },                     {                         "item\_id": 1696894,                         "sku": "228460\_22284600",                         "qty": 4                     }                 \]             }         }     } } |
| :---- |

## 23.5  Cật nhật item trong danh sách đặt hàng nhanh

**URL**: https://local.mmpro.vn/graphql  
**Method: POST**  
**Header:**

| Authorization : token\_customer login |
| :---- |

**Body:**

| mutation {  updateItemQuickOrder(    input: {      item\_id: "1696857"      qty: 1    }  ) {    success    message  } } |
| :---- |

**Param Description:**

- **item\_id:** lấy tại api get danh sách item  
- **qty:** số lượng muốn cập nhật

**Response:**

| {     "data": {         "updateItemQuickOrder": {             "success": true,             "message": "Cập nhật số lượng sản phẩm thành công"         }     } } |
| :---- |

## 23.6  Xóa một item trong danh sách đặt hàng nhanh

**URL**: https://local.mmpro.vn/graphql  
**Method: POST**  
**Header:**

| Authorization : token\_customer login |
| :---- |

**Body:**

| mutation {  removeItemQuickOrder(    input: {      item\_id: "1696860"    }  ) {    success    message  } } |
| :---- |

**Param Description:**

- **item\_id:** lấy tại api get danh sách item

**Response:**

| {     "data": {         "removeItemQuickOrder": {             "success": true,             "message": "Xóa sản phẩm thành công"         }     } } |
| :---- |

## 23.7  Xóa toàn bộ danh sách mua hàng nhanh

**URL**: https://local.mmpro.vn/graphql  
**Method: POST**  
**Header:**

| Authorization : token\_customer login |
| :---- |

**Body:**

| mutation {  removeQuickOrder(    input: {      customer\_id: "23667"    }  ) {    success    message  } } |
| :---- |

**Param Description:**

- **customer\_id:** lấy tại api get thông tin customer

**Response:**

| {     "data": {         "removeQuickOrder": {             "success": true,             "message": "Delete All Products successful"         }     } } |
| :---- |

## 23.8  Thêm toàn bộ danh sách mua hàng nhanh vào cart

**URL**: https://local.mmpro.vn/graphql  
**Method: POST**  
**Header:**

| Authorization : token\_customer login |
| :---- |

**Body:**

| mutation {  addAllItemQuickOrderToCart(    input: {      customer\_id: "23667"    }  ) {    success    message  } } |
| :---- |

**Param Description:**

- **customer\_id:** lấy tại api get thông tin customer

**Response:**

| {     "data": {         "addAllItemQuickOrderToCart": {             "success": true,             "message": "Thêm sản phẩm thành công"         }     } } |
| :---- |

## 23.9  Update ghi chú item quick order

**URL**: https://local.mmpro.vn/graphql  
**Method: POST**  
**Header:**

| Authorization : token\_customer login |
| :---- |

**Body:**

| mutation {  updateItemCommentQuickOrder(    input: {      item\_id: 1975868      comment: "1213"    }  ) {    success    message  } } |
| :---- |

**Response:**

| {     "data": {         "updateItemCommentQuickOrder": {             "success": true,             "message": "Cập nhật ghi chú sản phẩm thành công"         }     } } |
| :---- |

26. # 

27. # API FAQs

**URL Endpont**:   
https://local.mmpro.vn/graphql?query={faqs(store\_view\_code: "mm\_an\_phu\_vi", identifier: "chinh-sach-doi-tra") {category\_id is\_active name icon url\_key identifier content\_html\_page\_heade content\_html\_page\_footer faqs {id is\_active question answer}}}  
**Method: GET**  
**Param Description:**  
      **\-**  	store\_view\_code: Store view code  
**Response:**

| {     "data": {         "faqs": \[             {                 "category\_id": "1",                 "is\_active": 1,                 "name": "Giao Hàng",                 "icon": "fa-solid fa-truck",                 "url\_key": "giao-hàng",                 "faqs": \[                     {                         "id": "1",                         "is\_active": 0,                         "question": "Các sản phẩm nào MM Mega Market không áp dụng giao hàng?",                         "answer": "\<div data-content-type=\\"html\\" data-appearance=\\"default\\" data-element=\\"main\\"\>\&lt;div class=\\"vc\_toggle vc\_toggle\_default vc\_toggle\_color\_default vc\_toggle\_size\_md vc\_toggle\_active\\"\&gt;\\r\\n\&lt;div class=\\"vc\_toggle\_content\\"\&gt;\\r\\n\&lt;p\&gt;MM Mega Market không áp dụng giao hàng đối với các thực phẩm đông lạnh và mặt hàng sữa chua.\&lt;/p\&gt;\\r\\n\&lt;/div\&gt;\\r\\n\&lt;/div\&gt;\</div\>"                     },                     {                         "id": "2",                         "is\_active": 1,                         "question": "Khi nào đơn hàng của tôi được miễn phí vận chuyển ?",                         "answer": "\<div data-content-type=\\"html\\" data-appearance=\\"default\\" data-element=\\"main\\"\>\&lt;p\&gt;Các trung tâm MMVN sẽ cung cấp dịch vụ giao hàng miễn phí cho đơn hàng trong bán kính 10km và thỏa mãn những điều kiện sau đây:\&lt;/p\&gt;\\r\\n\&lt;ul style=\\"padding-left:25px\\"\&gt;\\r\\n\&lt;li aria-level=\\"1\\"\&gt;Đối với đơn hàng bao gồm mặt hàng thực phẩm tươi sống (\*): giá trị hóa đơn tối thiểu là 1.500.000 VNĐ/hóa đơn.\&lt;/li\&gt;\\r\\n\&lt;li aria-level=\\"1\\"\&gt;Đối với\&amp;nbsp; đơn hàng không bao gồm mặt hàng thực phẩm tươi sống (\*\*): giá trị hóa đơn tối thiểu là 1.000.000 vnđVNĐ/hóa đơn.\&lt;/li\&gt;\\r\\n\&lt;li aria-level=\\"1\\"\&gt;Trường hợp Quý khách ở phạm vi xa hơn 10km, MM Mega Market sẽ tính thêm phí vận chuyển với từng km tăng thêm và thay đổi theo từng khu vực. Nhân viên chăm sóc khách hàng sẽ liên hệ và trao đổi với khách hàng về phần chi phí giao hàng phát sinh.\&lt;/li\&gt;\\r\\n\&lt;/ul\&gt;\\r\\n\&lt;p\&gt;Lưu ý:\&lt;/p\&gt;\\r\\n\&lt;p\&gt;(\*) Không áp dụng giao hàng với các mặt hàng thực phẩm đông lạnh và sữa chua.\&lt;/p\&gt;\\r\\n\&lt;p\&gt;(\*\*) Các mặt hàng thực phẩm tươi sống bao gồm các thực phẩm tươi như thịt, cá, đồ đóng hộp, …\&lt;/p\&gt;\</div\>"                     },                     {                         "id": "3",                         "is\_active": 1,                         "question": "Thời gian giao hàng",                         "answer": "\<div data-content-type=\\"html\\" data-appearance=\\"default\\" data-element=\\"main\\"\>\&lt;ul style=\\"padding-left: 25px\\"\&gt;\\r\\n\&lt;li aria-level=\\"1\\"\&gt;Hàng hóa sẽ được giao trong vòng\&amp;nbsp;4\&amp;nbsp;giờ kể từ khi nhân viên CSKH xác nhận đơn đặt hàng (trừ trường hợp các ngày lễ và các trường hợp bất khả kháng, chúng tôi sẽ thông báo thời gian giao hàng cụ thể).\&amp;nbsp;\&lt;/li\&gt;\\r\\n\&lt;li aria-level=\\"1\\"\&gt;Thời gian nhận đơn hàng từ 08:00 giờ đến 17:00 giờ mỗi ngày.\&lt;/li\&gt;\\r\\n\&lt;li aria-level=\\"1\\"\&gt;Đối với những đơn hàng đặt sau 17:00 giờ sẽ được xử lý và giao vào ngày hôm sau.\&amp;nbsp;\&lt;/li\&gt;\\r\\n\&lt;/ul\&gt;\</div\>"                     }                 \]             },             {                 "category\_id": "5",                 "is\_active": 1,                 "name": "MM Mall",                 "icon": "fa-solid fa-bars",                 "url\_key": "mm-mall",                 "faqs": \[                     {                         "id": "13",                         "is\_active": 1,                         "question": "Quy trình thanh toán",                         "answer": "\<div data-content-type=\\"html\\" data-appearance=\\"default\\" data-element=\\"main\\"\>\&lt;p\&gt;\&amp;nbsp; \&amp;nbsp;\&lt;/p\&gt;\</div\>"                     }                 \]             },             {                 "category\_id": "2",                 "is\_active": 1,                 "name": "Đổi / Trả Hàng",                 "icon": "fa-solid fa-rotate-left",                 "url\_key": "Đổi-/-trả-hàng",                 "faqs": \[                     {                         "id": "24",                         "is\_active": 1,                         "question": "Muốn Trả Hàng / Hoàn Tiền thì phải làm sao?",                         "answer": "\<div data-content-type=\\"html\\" data-appearance=\\"default\\" data-element=\\"main\\"\>\&lt;p\&gt;\&amp;nbsp; \&amp;nbsp;\&lt;/p\&gt;\</div\>"                     }                 \]             }         \]     } } |
| :---- |

28. # API Product Label

**URL**: [https://local.mmpro.vn/graphql/](https://local.mmpro.vn/graphql/)  
**Body**:

| query {   products (     filter: { sku: { eq: "24-MB02"}}   ) {     items {             sku       name       product\_label {         label\_id         label\_name         label\_description         label\_priority         label\_status         label\_from\_date         label\_to\_date         label\_type         customer\_groups         stores         category\_image {           display           type           label\_size           shape\_color           shape\_type           text           text\_color           text\_font           text\_size           url           position           custom\_css         }         product\_image {           display           type           label\_size           shape\_color           shape\_type           text           text\_color           text\_font           text\_size           url           position           custom\_css         }       }      }   } } |
| :---- |

**Response**:

| {   "data": {     "products": {       "items": \[         {           "name": "English Course",           "sku": "English Course",           "product\_label": \[             {               "label\_id": "6",               "label\_description": null,               "label\_name": "test same",               "label\_status": 1,               "label\_from\_date": null,               "label\_to\_date": null,               "label\_priority": 1,               "label\_type": 1,               "stores": "0",               "customer\_groups": "0",               "product\_image": {                 "type": 3,                 "url": "http://course.local/media/label/tmp/image/Screenshot-9\_1.png",                 "position": "top-left",                 "display": 1,                 "text": null,                 "text\_color": "\#000000",                 "text\_font": null,                 "text\_size": 16,                 "shape\_type": null,                 "label\_size": "80",                 "label\_size\_mobile": "80",                 "custom\_css": "",                 "use\_default": 1,                 "picture\_frame": null               },               "category\_image": {                 "type": 3,                 "url": "http://course.local/media/label/tmp/image/Screenshot-9\_1.png",                 "position": "top-left",                 "display": 1,                 "text": null,                 "text\_color": "\#000000",                 "text\_font": null,                 "text\_size": 16,                 "shape\_type": null,                 "label\_size": "80",                 "label\_size\_mobile": "80",                 "custom\_css": "",                 "picture\_frame": null               }             }           \]         }       \]     }   } } |
| :---- |

29. # API Popup

    ## 26.1  Lấy thông tin Popup bằng category UID hoặc URL link

**URL:** [https://local.mmpro.vn/graphql/](https://local.mmpro.vn/graphql/)  
**Body:**

| query {    getPopup(category\_uid: "ODE5OQ==", link\_popup: "urllink") {        html\_content       css\_style       number\_x    } } |
| :---- |

**Param description:**   
“category\_uid”: UID của danh mục sẽ hiển thị popup  
“link\_popup”: url (không bao gồm base url) của popup. Nếu “category\_uid” bị bỏ trống sẽ sử dụng param này để query popup.

**Response:** “html\_content" và “css\_style” của các popup tương ứng

| {     "data": {         "getPopup": \[             {                 "html\_content": "\<div data-content-type=\\"html\\" data-appearance=\\"default\\" data-element=\\"main\\"\>\&lt;div class=\\"magenest-popup-step\\"\&gt;\\r\\n    \&lt;div class=\\"popup-step-1\\"\&gt;\\r\\n        \&lt;div class=\\"popup-content\\"\&gt;\\r\\n            \&lt;div class=\\"popup-title\\"\&gt;\\r\\n                \&lt;span\&gt;Would you like\&lt;/span\&gt;\\r\\n                \&lt;strong\&gt;50% Off\&lt;/strong\&gt;\\r\\n            \&lt;/div\&gt;\\r\\n            \&lt;div class=\\"popup-description\\"\&gt;\\r\\n                \&lt;p\&gt;on any autumn collections with this code:\&lt;/p\&gt;\\r\\n                \&lt;strong\&gt;New50\&lt;/strong\&gt;\\r\\n            \&lt;/div\&gt;\\r\\n            \&lt;div class=\\"popup-action\\"\&gt;\\r\\n                \&lt;div class=\\"action-yes\\" id=\\"yesbutton\\"\&gt;\\r\\n                    \&lt;button id=\\"popup-action-yes\\" class=\\"yes\\"\&gt;Yes Please\&lt;/button\&gt;\\r\\n                \&lt;/div\&gt;\\r\\n                \&lt;div class=\\"action-no\\" id=\\"no-button\\"\&gt;\\r\\n                    \&lt;button id=\\"popup-action-no\\" class=\\"no\\"\&gt;No Thanks\&lt;/button\&gt;\\r\\n                \&lt;/div\&gt;\\r\\n            \&lt;/div\&gt;\\r\\n        \&lt;/div\&gt;\\r\\n    \&lt;/div\&gt;\\r\\n\&lt;/div\&gt;\</div\>",                 "css\_style": "123",                 "number\_x": 1             }         \]     } } |
| :---- |

30. # API Cross-sell products

URL: [https://local.mmpro.vn/graphql/](https://local.mmpro.vn/graphql/)  
Body:

| query {   products (     filter: { sku: { eq: "217097\_22170972"}}   ) {     items {       sku       name       crosssell\_products {         sku         name         image {           label           url         }         stock\_status         special\_price         price\_range {           minimum\_price {             final\_price {               value               currency             }           }           maximum\_price {             final\_price {               value               currency             }           }         }       }     }   } } |
| :---- |

Response:

| {   "data": {     "products": {       "items": \[         {           "sku": "217097\_22170972",           "name": "SUA TH.TRUNG NG.CHAT TH 950ML",           "crosssell\_products": \[             {               "sku": "306319\_23063198",               "name": "LUOC MM PRO\*20CAI",               "image": {                 "label": "306319.jpg",                 "url": "https://local.mmpro247.vn/media/catalog/product/placeholder/default/logommvn-1000.jpg"               },               "stock\_status": "IN\_STOCK",               "special\_price": null,               "price\_range": {                 "minimum\_price": {                   "final\_price": {                     "value": 15741,                     "currency": "USD"                   }                 },                 "maximum\_price": {                   "final\_price": {                     "value": 15741,                     "currency": "USD"                   }                 }               }             },             {               "sku": "230796\_22307965",               "name": "NRT D.DA LAFFAIR H.DAU 5L",               "image": {                 "label": "230796.jpg",                 "url": "https://local.mmpro247.vn/media/catalog/product/placeholder/default/logommvn-1000.jpg"               },               "stock\_status": "IN\_STOCK",               "special\_price": null,               "price\_range": {                 "minimum\_price": {                   "final\_price": {                     "value": 223091,                     "currency": "USD"                   }                 },                 "maximum\_price": {                   "final\_price": {                     "value": 223091,                     "currency": "USD"                   }                 }               }             },             {               "sku": "230793\_22307934",               "name": "NRT D.DA LAFFAIR H.TAO 5L",               "image": {                 "label": "230793.jpg",                 "url": "https://local.mmpro247.vn/media/catalog/product/placeholder/default/logommvn-1000.jpg"               },               "stock\_status": "IN\_STOCK",               "special\_price": null,               "price\_range": {                 "minimum\_price": {                   "final\_price": {                     "value": 223091,                     "currency": "USD"                   }                 },                 "maximum\_price": {                   "final\_price": {                     "value": 223091,                     "currency": "USD"                   }                 }               }             },             {               "sku": "175806\_21758065",               "name": "NRT DR CLEAN D.DA H.TAO 500ML",               "image": {                 "label": "175806.jpg",                 "url": "https://local.mmpro247.vn/media/catalog/product/placeholder/default/logommvn-1000.jpg"               },               "stock\_status": "IN\_STOCK",               "special\_price": null,               "price\_range": {                 "minimum\_price": {                   "final\_price": {                     "value": 33636,                     "currency": "USD"                   }                 },                 "maximum\_price": {                   "final\_price": {                     "value": 33636,                     "currency": "USD"                   }                 }               }             },             {               "sku": "306261\_23062610",               "name": "DAO CAO RAU MM PRO\*20 CAI",               "image": {                 "label": "306261.jpg",                 "url": "https://local.mmpro247.vn/media/catalog/product/placeholder/default/logommvn-1000.jpg"               },               "stock\_status": "IN\_STOCK",               "special\_price": null,               "price\_range": {                 "minimum\_price": {                   "final\_price": {                     "value": 73636,                     "currency": "USD"                   }                 },                 "maximum\_price": {                   "final\_price": {                     "value": 73636,                     "currency": "USD"                   }                 }               }             }           \]         }       \]     }   } } |
| :---- |

31. # API Địa chỉ mặc định

**URL Endpont**:   
https://local.mmpro.vn/graphql?query={addressDefault {city\_code city\_name district\_code district\_name ward\_code ward\_name address}}  
**Method: GET**  
**Response:**

| {     "data": {         "addressDefault": {             "city\_code": "79",             "city\_name": "Thành phố Hồ Chí Minh",             "district\_code": "760",             "district\_name": "Quận 1",             "ward\_code": "26740",             "ward\_name": "Phường Bến Nghé",             "address": "Số 9"         }     } } |
| :---- |

**Note:** Config địa chỉ mặc định tại: Stores \-\> Configurable \-\> Magenest \-\> Setting Address Default

32. # API Xóa All Items Cart

**URL**: https://local.mmpro.vn/graphql  
**Method: POST**  
**Header:**

| Authorization : token\_customer login |
| :---- |

**Body:**

| mutation {  removeAllCartItems(    input: {      cart\_id: "gZdLk7q3nX2Q1rPDvOQcpg1rMcbdvoH4"    }  ) {  success } } |
| :---- |

**Param Description:**

- **cart\_id:** cart id

**Response:**

| {     "data": {         "removeAllCartItems": {             "success": true         }     } } |
| :---- |

33. # API Flash Sale

    ## 31.1  Flash sale homepage

**URL**: https://local.mmpro.vn/graphql?query={getFlashSaleProducts(pageSize: 5\) {end\_time items {id uid name description{html \_\_typename}price\_range{maximum\_price{final\_price{currency value \_\_typename}regular\_price{currency value \_\_typename}discount{amount\_off \_\_typename}\_\_typename}\_\_typename}sku small\_image{url \_\_typename}stock\_status \_\_typename url\_key} total\_count}}  
**Method: GET**  
**Param Description:**  
      **\-**  	end\_time: Thời gian kết thúc flashsale 

- items: danh sách product flashsale  
- total\_count: số lượng item product trả về

**Response:**

| {     "data": {         "getFlashSaleProducts": {             "end\_time": "2024-10-31 09:48:00",             "items": \[                 {                     "id": 70240,                     "uid": "NzAyNDA=",                     "name": "DUI GO BO TINH-LOAI 1",                     "description": {                         "html": "",                         "\_\_typename": "ComplexTextValue"                     },                     "price\_range": {                         "maximum\_price": {                             "final\_price": {                                 "currency": "VND",                                 "value": 362809.75,                                 "\_\_typename": "Money"                             },                             "regular\_price": {                                 "currency": "VND",                                 "value": 381905,                                 "\_\_typename": "Money"                             },                             "discount": {                                 "amount\_off": 19095.25,                                 "\_\_typename": "ProductDiscount"                             },                             "\_\_typename": "ProductPrice"                         },                         "\_\_typename": "PriceRange"                     },                     "sku": "2297\_20022976",                     "small\_image": {                         "url": "https://local.mmpro.vn/static/version1729494311/frontend/Smartosc/MM/vi\_VN/Magento\_Catalog/images/product/placeholder/small\_image.jpg",                         "\_\_typename": "ProductImage"                     },                     "stock\_status": "IN\_STOCK",                     "\_\_typename": "SimpleProduct",                     "url\_key": "beef-knuckle-boneless-70240"                 },                 {                     "id": 268321,                     "uid": "MjY4MzIx",                     "name": "DUI GO BO TINH-L",                     "description": {                         "html": "",                         "\_\_typename": "ComplexTextValue"                     },                     "price\_range": {                         "maximum\_price": {                             "final\_price": {                                 "currency": "VND",                                 "value": 158800.2,                                 "\_\_typename": "Money"                             },                             "regular\_price": {                                 "currency": "VND",                                 "value": 264667,                                 "\_\_typename": "Money"                             },                             "discount": {                                 "amount\_off": 105866.8,                                 "\_\_typename": "ProductDiscount"                             },                             "\_\_typename": "ProductPrice"                         },                         "\_\_typename": "PriceRange"                     },                     "sku": "373126\_23731264",                     "small\_image": {                         "url": "https://local.mmpro.vn/static/version1729494311/frontend/Smartosc/MM/vi\_VN/Magento\_Catalog/images/product/placeholder/small\_image.jpg",                         "\_\_typename": "ProductImage"                     },                     "stock\_status": "IN\_STOCK",                     "\_\_typename": "SimpleProduct",                     "url\_key": "dui-go-bo-tinh-loai-23731264-373126"                 },                 {                     "id": 115598,                     "uid": "MTE1NTk4",                     "name": "DUI GO BO NGUYEN KHOI-PC",                     "description": {                         "html": "",                         "\_\_typename": "ComplexTextValue"                     },                     "price\_range": {                         "maximum\_price": {                             "final\_price": {                                 "currency": "VND",                                 "value": 209286,                                 "\_\_typename": "Money"                             },                             "regular\_price": {                                 "currency": "VND",                                 "value": 279048,                                 "\_\_typename": "Money"                             },                             "discount": {                                 "amount\_off": 69762,                                 "\_\_typename": "ProductDiscount"                             },                             "\_\_typename": "ProductPrice"                         },                         "\_\_typename": "PriceRange"                     },                     "sku": "382601\_23826014",                     "small\_image": {                         "url": "https://local.mmpro.vn/static/version1729494311/frontend/Smartosc/MM/vi\_VN/Magento\_Catalog/images/product/placeholder/small\_image.jpg",                         "\_\_typename": "ProductImage"                     },                     "stock\_status": "IN\_STOCK",                     "\_\_typename": "SimpleProduct",                     "url\_key": "dui-go-bo-nguyen-kho-115598"                 },                 {                     "id": 70241,                     "uid": "NzAyNDE=",                     "name": "DUI GO BO LOC \- TN",                     "description": {                         "html": "",                         "\_\_typename": "ComplexTextValue"                     },                     "price\_range": {                         "maximum\_price": {                             "final\_price": {                                 "currency": "VND",                                 "value": 280286.1,                                 "\_\_typename": "Money"                             },                             "regular\_price": {                                 "currency": "VND",                                 "value": 311429,                                 "\_\_typename": "Money"                             },                             "discount": {                                 "amount\_off": 31142.9,                                 "\_\_typename": "ProductDiscount"                             },                             "\_\_typename": "ProductPrice"                         },                         "\_\_typename": "PriceRange"                     },                     "sku": "351103\_23511033",                     "small\_image": {                         "url": "https://local.mmpro.vn/static/version1729494311/frontend/Smartosc/MM/vi\_VN/Magento\_Catalog/images/product/placeholder/small\_image.jpg",                         "\_\_typename": "ProductImage"                     },                     "stock\_status": "IN\_STOCK",                     "\_\_typename": "SimpleProduct",                     "url\_key": "beef-knuckle-boneless-70241"                 }             \],             "total\_count": 4         }     } } |
| :---- |

34. # API giờ và ghi chú giao hàng

    ## 33.1  Lấy config ngày giao hàng

**URL**: https://local.mmpro.vn/graphql  
**Method: POST**  
**Header:**

| Store: store\_view\_code |
| :---- |

**Body:**

| {    getDeliveryDateConfiguration {        id        enabled        schedules {            schedule\_id            value            label        }    } } |
| :---- |

**Response:**

| {     "data": {         "getDeliveryDateConfiguration": \[             {                 "id": 1,                 "enabled": 1,                 "schedules": \[                     {                         "schedule\_id": 1,                         "value": "05/11/2024",                         "label": "2024-11-05"                     },                     {                         "schedule\_id": 3,                         "value": "06/11/2024",                         "label": "2024-11-06"                     },                     {                         "schedule\_id": 4,                         "value": "07/11/2024",                         "label": "2024-11-07"                     },                     {                         "schedule\_id": 5,                         "value": "08/11/2024",                         "label": "2024-11-08"                     },                     {                         "schedule\_id": 6,                         "value": "09/11/2024",                         "label": "2024-11-09"                     }                 \]             }         \]     } }               |
| :---- |

## 33.2  Lấy config giờ giao hàng theo ngày giao hàng

**URL**: https://local.mmpro.vn/graphql  
**Method: POST**  
**Header:**

| Store: store\_view\_code |
| :---- |

**Body:**

| {    getTimeInterval(schedule\_id: 2, date: "2024-11-19") {        time\_interval\_id        from        to        label    } } |
| :---- |

**Response:**

| {     "data": {         "getTimeInterval": \[             {                 "time\_interval\_id": 4,                 "from": "420",                 "to": "540",                 "label": "You're an early bird\! Please note, this is a high-demand delivery slot. The deliveries might move a bit."             },             {                 "time\_interval\_id": 5,                 "from": "540",                 "to": "660",                 "label": ""             },             {                 "time\_interval\_id": 6,                 "from": "660",                 "to": "840",                 "label": ""             }         \]     } }             |
| :---- |

\- **Note**: Giá trị “from" và “to” của “time\_interval” hiện tại đang là giá trị phút trong 1 ngày 24\. Sẽ lấy value đó chia cho 60 \-\> giờ trong ngày.   
Ví dụ from \= 420 và to \= 540 \-\> from \= 7AM \- to \= 9AM

## 33.3  Set giá trị thời gian và ghi chú giao hàng cho quote và order

**URL**: https://local.mmpro.vn/graphql  
**Method: POST**  
**Header:**

| Authorization : token\_customer login Store: store\_view\_code |
| :---- |

**Body:**

| mutation {  setShippingMethodsOnCart(input: {    cart\_id: "UhKUgBO6ecf9fbOVoxEAtSwEgeNa6xRZ"    shipping\_methods: \[      {        carrier\_code: "flatrate"        method\_code: "flatrate"      }    \]    delivery\_date: {        date: "2024-11-22"        time\_interval\_id: 4        comment: "comment test 22"    }  }) {    cart {      shipping\_addresses {        selected\_shipping\_method {          carrier\_code          method\_code          carrier\_title          method\_title        }      }    }  } } |
| :---- |

**Param Request Description:**

- **delivery\_date:** truyền vào 3 trường ngày, thời gian, ghi chú  
+ **date:** string \- value chọn ngày  
+ **time\_interval\_id:** \- int: value option chọn giờ id lấy ở api get config thời gian  
+ **comment:** string \- value trường comment

35. # API theo dõi đơn hàng

**URL**: [https://local.mmpro.vn/graphql/](https://local.mmpro.vn/graphql/)  
**Body**:

| {    orderTracking(        order\_number: "37000057407"        email: "9d36a744@example.com"    ) {        id        status        number        email        order\_date        increment\_id        grand\_total        created\_at        shipping\_method        carrier        total {            subtotal {                value                currency            }            grand\_total {                value                currency            }            total\_shipping {                value                currency            }            discounts {                amount {                value                currency                }            }        }        shipping\_address {            firstname            lastname            telephone            postcode            street            city        }        items {            product\_sku            product\_name            quantity\_ordered            product\_sale\_price {                value                currency            }            product {                sku                name                unit\_ecom                image{                    label                    url                }            }        }    } } |
| :---- |

**Response:**

| {     "data": {         "orderTracking": {             "id": "Nzg5ODMy",             "status": "Đợi duyệt",             "number": "37000057407",             "email": "9d36a744@example.com",             "order\_date": "2024-09-05 12:38:12",             "increment\_id": "37000057407",             "grand\_total": 1990136,             "created\_at": "2024-09-05 12:38:12",             "shipping\_method": "flatrate\_flatrate",             "carrier": "Flat Rate",             "total": {                 "subtotal": {                     "value": 2296534,                     "currency": "VND"                 },                 "grand\_total": {                     "value": 1990136,                     "currency": "VND"                 },                 "total\_shipping": {                     "value": 0,                     "currency": "VND"                 },                 "discounts": \[                     {                         "amount": {                             "value": 306398,                             "currency": "VND"                         }                     }                 \]             },             "shipping\_address": {                 "firstname": "TRUONG MAM NON HAI VIEN",                 "lastname": "TRUONG MAM NON HAI VIEN",                 "telephone": "0936581028",                 "postcode": "70000",                 "street": \[                     "LO 3C KHU DO THI MOI NGA5 SAN BAY CATBI, P DONGKHE Q NQUYEN"                 \],                 "city": "Thành phố Hải Phòng"             },             "items": \[                 {                     "product\_sku": "36090\_20360900",                     "product\_name": "Cải thảo xuất xứ Trung Quốc loại 1",                     "quantity\_ordered": 4,                     "product\_sale\_price": {                         "value": 18612,                         "currency": "VND"                     },                     "product": {                         "sku": "36090\_20360900",                         "name": "CAI THAO XX TRUNG QUOC \-LOAI 1",                         "unit\_ecom": "Kg",                         "image": {                             "label": "36090.jpg",                             "url": "https://local.mmpro.vn/static/version1730276198/frontend/Smartosc/MM/vi\_VN/Magento\_Catalog/images/product/placeholder/image.jpg"                         }                     }                 },                 {                     "product\_sku": "320514\_23205147",                     "product\_name": "CA HOI NAUY WAF (FILLET-CDA-TUOI)",                     "quantity\_ordered": 4.2,                     "product\_sale\_price": {                         "value": 529068,                         "currency": "VND"                     },                     "product": {                         "sku": "320514\_23205147",                         "name": "CA HOI NAUY WRF (FILLET-CDA-TUOI)",                         "unit\_ecom": "Kg",                         "image": {                             "label": "320514.jpg",                             "url": "https://local.mmpro.vn/static/version1730276198/frontend/Smartosc/MM/vi\_VN/Magento\_Catalog/images/product/placeholder/image.jpg"                         }                     }                 }             \]         }     } } |
| :---- |

**Param Description:**  
      **\-**  	order\_number: mã đơn hàng (increment id)

- email: email thanh toán đặt hàng theo đơn hàng

36. # API cancel đơn hàng	

Chỉ áp dụng cho customer.   
**URL**: [https://local.mmpro.vn/graphql/](https://local.mmpro.vn/graphql/)  
**Body**:

| mutation {     cancelOrder(         input: {             order\_id: "99999999",             reason: "The order was placed by mistake"         }     ){         error         order {             status         }     } }  |
| :---- |

**Response:**

| {   "data": {     "cancelOrder": {       "error": null,       "order": {         "status": "Canceled"       }     }   }  |
| :---- |

37. # API hóa đơn công ty VAT

    ## 36.1  Lấy thông tin hóa đơn có sẵn theo customer

**URL**: https://local.mmpro.vn/graphql  
**Method: POST**  
**Header:**

| Authorization : token\_customer login |
| :---- |

**Body:**

| {    vatInformation {        customer\_vat\_id        company\_name        company\_vat\_number        company\_address    } } |
| :---- |

**Response:**

| {     "data": {         "vatInformation": {             "customer\_vat\_id": 1,             "company\_name": "THC company 5",             "company\_vat\_number": "9999999999",             "company\_address": "Hoai Duc, Ha Noi, Viet Nam"         }     } } |
| :---- |

## 36.2  Set thông tin hóa đơn trước checkout (place order)

**URL**: https://local.mmpro.vn/graphql  
**Method: POST**  
**Header:**

| Authorization : token\_customer login |
| :---- |

**Chú ý:** Trường “customer\_vat\_id” lấy ở api get thông tin hóa đơn. khi khách hàng chưa checkout lần nào hoặc chưa có hóa đơn công ty thì sẽ truyền vào \= null.  
**Body:**

| mutation {  setVatInformationOnCart(    input: {      cart\_id: "46g3iOtku000I5UnR13jHpehi1VkpKOO"      vat\_address:        {            customer\_vat\_id: 2            company\_name: "THC company 9"            company\_vat\_number: "9999999999"            company\_address: "Hoai Duc, Ha Noi, Viet Nam"        }    }  ) {    cart {      vat\_address {        customer\_vat\_id        company\_name        company\_vat\_number        company\_address      }    }  } } |
| :---- |

**Response:**

| {     "data": {         "setVatInformationOnCart": {             "cart": {                 "vat\_address": {                     "customer\_vat\_id": 2,                     "company\_name": "THC company 9",                     "company\_vat\_number": "9999999999",                     "company\_address": "Hoai Duc, Ha Noi, Viet Nam"                 }             }         }     } } |
| :---- |

## 36.3  Cập nhật thông tin VAT customer my account

**URL**: https://local.mmpro.vn/graphql  
**Method: POST**  
**Header:**

| Authorization : token\_customer login |
| :---- |

**Body:**

| mutation {  updateCustomer(    input: {      firstname: "Rob",      customer\_no: "123456789",      vat\_address: {        company\_name: "Magenest"        company\_vat\_number: "999999"        company\_address: "Hà Nội"      }    }  ) {    customer {      firstname      customer\_no      vat\_address {        customer\_vat\_id        company\_name        company\_vat\_number        company\_address    }    }  } } |
| :---- |

**Response:**

| {     "data": {         "updateCustomer": {             "customer": {                 "firstname": "Rob",                 "customer\_no": "123456789",                 "vat\_address": {                     "customer\_vat\_id": 4,                     "company\_name": "Magenest",                     "company\_vat\_number": "999999",                     "company\_address": "Hà Nội"                 }             }         }     } } |
| :---- |

38. # API Tạo wishlist

URL: [https://local.mmpro.vn/graphql](https://local.mmpro.vn/graphql)  
Cần user token để có thể sử dụng api wishlist. Thêm authorization với bearer token.

| mutation {   generateCustomerToken (     email: "c9c4967433\_356@example.com"     password: "Phupd23500"   ) {     token   } } |
| :---- |

Body:

| mutation {   createWishlist (     input: { name: "test multiple"}   ) {     wishlist {       wishlist\_id       name       customer\_id     }   } } |
| :---- |

Param:

- name: tên của wishlist

Response:

| {   "data": {     "createWishlist": {       "wishlist": {         "wishlist\_id": "9701",         "name": "test multiple",         "customer\_id": 356       }     }   } } |
| :---- |

Response description: 

- wishlist\_id: id của wishlist  
- name: tên của wishlist  
- customer\_id: id của customer

39. # API Xoá wishlist

URL: [https://local.mmpro.vn/graphql](https://local.mmpro.vn/graphql)  
Cần user token để có thể sử dụng api wishlist. Thêm authorization với bearer token.

| mutation {   generateCustomerToken (     email: "c9c4967433\_356@example.com"     password: "Phupd23500"   ) {     token   } } |
| :---- |

Body:

| mutation {   deleteWishlist (     wishlistId: 9693   ) {     status   } } |
| :---- |

Param description:

- wishlistId: id của wishlist cần xoá

Response:

| {   "data": {     "deleteWishlist": {       "status": true     }   } } |
| :---- |

Response description:

- status: trạng thái của action (true/false đại diện cho có xoá được hay không)

40. # API Đổi tên wishlist

URL: [https://local.mmpro.vn/graphql](https://local.mmpro.vn/graphql)  
Cần user token để có thể sử dụng api wishlist. Thêm authorization với bearer token.

| mutation {   generateCustomerToken (     email: "c9c4967433\_356@example.com"     password: "Phupd23500"   ) {     token   } } |
| :---- |

Body:

| mutation {   updateWishlist (     wishlistId: 12     name: "test update wishlist"   ) {     wishlistId     name     status   } } |
| :---- |

Param description:

- wishlistId: id của wishlist cần đổi tên  
- name: tên của wishlist muốn thay đổi thành

Response:

| {   "data": {     "updateWishlist": {       "wishlistId": "9701",       "name": "test update wishlist",       "status": true     }   } } |
| :---- |

Response description: 

- wishlistId: id của wishlist  
- name: tên sau khi thay đổi  
- status: trạng thái của action (true/false)

41. # API ghi chú sản phẩm

1. **UpdateCommentOnCartItem**  
   

**Syntax**

| mutation: {updateCommentOnCartItem(input: UpdateCommentOnCartItemInput): {UpdateCommentOnCartItemOutput}} |
| :---- |

**Arguments:**

| cart\_id \- String\! | The unique ID of a Cart object. |
| :---- | :---- |
| cart\_item\_uid \- ID\! | The unique ID for a CartItemInterface object. |
| comment\! | Comment for item |

**Request** 

| mutation{ updateCommentOnCartItem(input: {   cart\_id: "KqwOQ0yPa5AXgODn6UTl7NoZHMXlNGwK"   cart\_item\_uid:"NDIyMjIxOQ=="   comment:"thao comment" }){   cart {   id   items {     uid     comment   } } }} |
| :---- |

**Response:**

| {  "data": {    "updateCommentOnCartItem": {      "cart": {        "id": "KqwOQ0yPa5AXgODn6UTl7NoZHMXlNGwK",        "items": \[          {            "uid": "NDIyMjIxOQ==",            "comment": "thao comment"          }        \]      }    }  }} |
| :---- |

2. **removeCommentFromCartItem**

	  
**Syntax**

| mutation: {removeCommentFromCartItem(input: RemoveCommentFromCartItemInput): {RemoveCommentFromCartItemOutput}} |
| :---- |

**Arguments:**

| cart\_id \- String\! | The unique ID of a Cart object. |
| :---- | :---- |
| cart\_item\_uid \- ID\! | The unique ID for a CartItemInterface object. |

**Request** 

| mutation {  removeCommentFromCartItem(input: {      cart\_id: "KqwOQ0yPa5AXgODn6UTl7NoZHMXlNGwK",      cart\_item\_uid: "NDIyMjIxOQ=="    }) {    cart {      id      items {        uid        comment      }    }  }} |
| :---- |

**Response:**

| {  "data": {    "removeCommentFromCartItem": {      "cart": {        "id": "KqwOQ0yPa5AXgODn6UTl7NoZHMXlNGwK",        "items": \[          {            "uid": "NDIyMjIxOQ==",            "comment": ""          }        \]      }    }  }} |
| :---- |

42. # API danh sách wishlist của customer

URL: [https://local.mmpro.vn/graphql](https://local.mmpro.vn/graphql)  
Body: 

| query {  customer {    wishlists {      id      name      items\_count      items\_v2 {        items {          id          description          quantity          product {            sku            name          }        }        page\_info {          current\_page          page\_size          total\_pages        }      }      updated\_at    }  } } |
| :---- |

Param description  
Response:

| {    "data": {        "customer": {            "wishlists": \[                {                    "id": "12",                    "name": "Danh sách mong muốn",                    "items\_count": 2,                    "items\_v2": {                        "items": \[                            {                                "id": "33786",                                "description": null,                                "quantity": 1,                                "product": {                                    "sku": "258956\_22589569",                                    "name": "BO VIEN BIGMEAL 500G"                                }                            },                            {                                "id": "33787",                                "description": null,                                "quantity": 1,                                "product": {                                    "sku": "258956\_22589569",                                    "name": "BO VIEN BIGMEAL 500G"                                }                            }                        \],                        "page\_info": {                            "current\_page": 1,                            "page\_size": 20,                            "total\_pages": 1                        }                    },                    "updated\_at": "2021-03-11 01:04:39"                },                {                    "id": "9701",                    "name": "test update wishlist",                    "items\_count": 0,                    "items\_v2": {                        "items": \[\],                        "page\_info": {                            "current\_page": 1,                            "page\_size": 20,                            "total\_pages": 1                        }                    },                    "updated\_at": "2024-11-01 10:18:29"                },                {                    "id": "9702",                    "name": "update 1",                    "items\_count": 0,                    "items\_v2": {                        "items": \[\],                        "page\_info": {                            "current\_page": 1,                            "page\_size": 20,                            "total\_pages": 1                        }                    },                    "updated\_at": "2024-11-04 02:08:16"                },                {                    "id": "9705",                    "name": "test multiple",                    "items\_count": 0,                    "items\_v2": {                        "items": \[\],                        "page\_info": {                            "current\_page": 1,                            "page\_size": 20,                            "total\_pages": 1                        }                    },                    "updated\_at": "2024-11-04 02:19:45"                }            \]        }    } } |
| :---- |

43. # API địa chỉ customer

    ## 42.1  Set địa chỉ tại checkout

**URL**: https://local.mmpro.vn/graphql  
**Method: POST**  
**Header:**

| Authorization : token\_customer login |
| :---- |

**Body:**

| mutation {  setShippingAddressesOnCart(    input: {      cart\_id: "dFXN6EDxnEx2dx54u3Qv9XEhaKHGKkga"      shipping\_addresses: \[        {          address: {            firstname: "Ta Huu"            lastname: ""            street: \["Magento Pkwy"\]            city: "Thành phố Hồ Chí Minh"            city\_code: "79"            district\_code: "760"            ward\_code: "26740"            country\_code: "VN"            telephone: "8675309"            save\_in\_address\_book: true          }        }      \]    }  ) {    cart {      shipping\_addresses {        firstname        lastname        street        city        telephone        country {          code          label        }      }    }  } } |
| :---- |

**Response:**

| {     "data": {         "setShippingAddressesOnCart": {             "cart": {                 "shipping\_addresses": \[                     {                         "firstname": "Ta Huu",                         "middlename": "",                         "lastname": "Cuong",                         "prefix": "Mr.",                         "suffix": "Jr.",                         "company": "Magento",                         "street": \[                             "Magento Pkwy"                         \],                         "city": "Thành phố Hồ Chí Minh",                         "region": {                             "code": "VN79",                             "label": "Thành phố Hồ Chí Minh"                         },                         "postcode": "78758",                         "telephone": "8675309",                         "fax": "8675311",                         "country": {                             "code": "VN",                             "label": "VN"                         }                     }                 \]             }         }     } } |
| :---- |

## 42.2  Get danh sách địa chỉ

**URL**: https://local.mmpro.vn/graphql  
**Method: POST**  
**Header:**

| Authorization : token\_customer login |
| :---- |

**Body:**

| { customer {   addressesV2(     currentPage: 1     pageSize: 20   ) {        total\_count        total\_pages        addresses {            id             default\_shipping            firstname            lastname            street            city            country\_code            telephone            company            custom\_attributes {                attribute\_code                value            }        }   } } } |
| :---- |

**Response:**

| {     "data": {         "customer": {             "addressesV2": {                 "total\_count": 5,                 "total\_pages": 1,                 "addresses": \[                     {                         "id": 75871,                         "default\_shipping": false,                         "firstname": "thuy.phan91212@gmail.com",                         "lastname": "thuy.phan91212@gmail.com",                         "street": \[                             "1 Ly Tu Trong, P. Ben Nghe, Q. 1, TP. Ho Chi Minh"                         \],                         "city": "Thành phố Hồ Chí Minh",                         "country\_code": "VN",                         "telephone": "0933724941",                         "company": "CONG TY CO PHAN IMPERIAL CARE",                         "custom\_attributes": \[                             {                                 "attribute\_code": "mm\_address\_id",                                 "value": "7664915"                             }                         \]                     },                     {                         "id": 75872,                         "default\_shipping": false,                         "firstname": "thuy.phan91212@gmail.com",                         "lastname": "thuy.phan91212@gmail.com",                         "street": \[                             "1 Ly Tu Trong, P. Ben Nghe, Q. 1, TP. Ho Chi Minh"                         \],                         "city": "Thành phố Hồ Chí Minh",                         "country\_code": "VN",                         "telephone": "0933724941",                         "company": "CONG TY CO PHAN IMPERIAL CARE",                         "custom\_attributes": \[                             {                                 "attribute\_code": "mm\_address\_id",                                 "value": "7664916"                             },                             {                                 "attribute\_code": "city\_code",                                 "value": "79"                             },                             {                                 "attribute\_code": "district",                                 "value": "Quận 1"                             },                             {                                 "attribute\_code": "district\_code",                                 "value": "760"                             },                             {                                 "attribute\_code": "ward",                                 "value": "Phường Bến Nghé"                             },                             {                                 "attribute\_code": "ward\_code",                                 "value": "26740"                             }                         \]                     },                     {                         "id": 75889,                         "default\_shipping": true,                         "firstname": "Thìn 2",                         "lastname": "",                         "street": \[                             "Số 23 ngách 5/72, Ngõ 5 Cầu Giấy, Dịch Vọng, Cầu Giấy, Hà Nội"                         \],                         "city": "Thành phố Hà Nội",                         "country\_code": "VN",                         "telephone": "0384624572",                         "company": "Magenest 2",                         "custom\_attributes": \[                             {                                 "attribute\_code": "city\_code",                                 "value": "1"                             },                             {                                 "attribute\_code": "district",                                 "value": "Quận Cầu Giấy"                             },                             {                                 "attribute\_code": "district\_code",                                 "value": "5"                             },                             {                                 "attribute\_code": "ward",                                 "value": "Phường Dịch Vọng"                             },                             {                                 "attribute\_code": "ward\_code",                                 "value": "166"                             }                         \]                     },                     {                         "id": 75890,                         "default\_shipping": false,                         "firstname": "Tạ Hữu Cường",                         "lastname": "",                         "street": \[                             "Số 23 ngách 5/72, Ngõ 5 Cầu Giấy, Dịch Vọng, Cầu Giấy, Hà Nội"                         \],                         "city": "Thành phố Hà Nội",                         "country\_code": "VN",                         "telephone": "0384624572",                         "company": "Magenest",                         "custom\_attributes": \[                             {                                 "attribute\_code": "city\_code",                                 "value": "1"                             },                             {                                 "attribute\_code": "district",                                 "value": "Quận Cầu Giấy"                             },                             {                                 "attribute\_code": "district\_code",                                 "value": "5"                             },                             {                                 "attribute\_code": "ward",                                 "value": "Phường Dịch Vọng"                             },                             {                                 "attribute\_code": "ward\_code",                                 "value": "166"                             }                         \]                     },                     {                         "id": 75891,                         "default\_shipping": false,                         "firstname": "Tạ Hữu Cường",                         "lastname": "",                         "street": \[                             "Số 23 ngách 5/72, Ngõ 5 Cầu Giấy, Dịch Vọng, Cầu Giấy, Hà Nội"                         \],                         "city": "Thành phố Hà Nội",                         "country\_code": "VN",                         "telephone": "0384624572",                         "company": "Magenest",                         "custom\_attributes": \[                             {                                 "attribute\_code": "city\_code",                                 "value": "1"                             },                             {                                 "attribute\_code": "district",                                 "value": "Quận Cầu Giấy"                             },                             {                                 "attribute\_code": "district\_code",                                 "value": "5"                             },                             {                                 "attribute\_code": "ward",                                 "value": "Phường Dịch Vọng"                             },                             {                                 "attribute\_code": "ward\_code",                                 "value": "166"                             }                         \]                     }                 \]             }         }     } } |
| :---- |

**Note:** Lấy tỉnh thành/phố, quận/huyện, phường/xã value tại mục custom\_attributes, city\_code, district\_code, ward\_code.

## 42.3  Edit địa chỉ

**URL**: https://local.mmpro.vn/graphql  
**Method: POST**  
**Header:**

| Authorization : token\_customer login |
| :---- |

**Body:**

| mutation { updateCustomerAddress(id:75911, input: {   country\_code: VN   street: \["Số 23 ngách 5/72, Ngõ 5 Cầu Giấy, Dịch Vọng, Cầu Giấy, Hà Nội"\]   telephone: "0384624572"   firstname: "Thìn 2"   city: "1"   default\_shipping: true   custom\_attributes: \[           {               attribute\_code: "city\_code",               value: "1"           },           {               attribute\_code: "district\_code",               value: "5"           },           {               attribute\_code: "ward\_code",               value: "166"           }           \] }) {    id      default\_shipping     firstname     lastname     street     city     country\_code     telephone     custom\_attributes {     attribute\_code      value    } } } |
| :---- |

**Response:**

| {     "data": {         "updateCustomerAddress": {             "id": 75911,             "default\_shipping": true,             "firstname": "Thìn 2",             "lastname": "",             "street": \[                 "Số 23 ngách 5/72, Ngõ 5 Cầu Giấy, Dịch Vọng, Cầu Giấy, Hà Nội"             \],             "city": "Thành phố Hà Nội",             "country\_code": "VN",             "telephone": "0384624572",             "custom\_attributes": \[                 {                     "attribute\_code": "city\_code",                     "value": "1"                 },                 {                     "attribute\_code": "district",                     "value": "Quận Cầu Giấy"                 },                 {                     "attribute\_code": "district\_code",                     "value": "5"                 },                 {                     "attribute\_code": "ward",                     "value": "Phường Dịch Vọng"                 },                 {                     "attribute\_code": "ward\_code",                     "value": "166"                 }             \]         }     } } |
| :---- |

## 42.4  Thêm địa chỉ

**URL**: https://local.mmpro.vn/graphql  
**Method: POST**  
**Header:**

| Authorization : token\_customer login |
| :---- |

**Body:**

| mutation { createCustomerAddress(input: {   country\_code: VN   street: \["Số 23 ngách 5/72, Ngõ 5 Cầu Giấy, Dịch Vọng, Cầu Giấy, Hà Nội"\]   telephone: "0384624572"   city: "1"   firstname: "Tạ Hữu Cường"   default\_shipping: false   custom\_attributes: \[           {               attribute\_code: "city\_code",               value: "1"           },           {               attribute\_code: "district\_code",               value: "5"           },           {               attribute\_code: "ward\_code",               value: "166"           }           \] }) {   id   country\_code   street   telephone   city   default\_shipping   default\_billing   custom\_attributes {   attribute\_code   value   } } } |
| :---- |

**Response:**

| {     "data": {         "createCustomerAddress": {             "id": 75911,             "country\_code": "VN",             "street": \[                 "Số 23 ngách 5/72, Ngõ 5 Cầu Giấy, Dịch Vọng, Cầu Giấy, Hà Nội"             \],             "telephone": "0384624572",             "city": "Thành phố Hà Nội",             "default\_shipping": false,             "default\_billing": false,             "custom\_attributes": \[                 {                     "attribute\_code": "city\_code",                     "value": "1"                 },                 {                     "attribute\_code": "district\_code",                     "value": "5"                 },                 {                     "attribute\_code": "ward\_code",                     "value": "166"                 },                 {                     "attribute\_code": "district",                     "value": "Quận Cầu Giấy"                 },                 {                     "attribute\_code": "ward",                     "value": "Phường Dịch Vọng"                 }             \]         }     } } |
| :---- |

44. # API bổ sung order detail

URL: [https://local.mmpro.vn/graphql](https://local.mmpro.vn/graphql)  
Header: Authorization: Bearer \<token\_customer\>  
Body:

| query {   customer {     orders(filter: { number: {eq: "26000048107"}}) {       items {         number         customer\_no         delivery\_information {           delivery\_date           delivery\_from           delivery\_to         }         vat\_information {           company\_address           company\_name           company\_vat\_number           customer\_vat\_id         }        }     }    } } |
| :---- |

Param description:

- 26000048107: order increment\_id.

Response:

| {   "data": {     "customer": {       "orders": {         "items": \[           {             "number": "26000048107",             "customer\_no": "2219001353865",             "delivery\_information": {               "delivery\_date": "2024-01-01",               "delivery\_from": "12:00",               "delivery\_to": "14:00"             },             "vat\_information": {               "company\_address": "123 la thành, ô chợ dừa, hà ",               "company\_name": "Công ty TNHH ABC",               "company\_vat\_number": "1234567890",               "customer\_vat\_id": null             }           }         \]       }     }   } } |
| :---- |

Response description:

- customer\_no: mã khách hàng.  
- delivery\_information: menu cấp 1  
  - delivery\_date: ngày giao hàng  
  - delivery\_from: giờ bắt đầu giao hàng  
  - delivery\_to: giờ kết thúc giao hàng  
    

45. # API set mã Mcard checkout

**URL**: https://local.mmpro.vn/graphql  
**Method: POST**  
**Header:**

| Authorization : token\_customer login |
| :---- |

**Body:**

| mutation {  setCustomerNoOnCart(    input: {      cart\_id: "HEEPCqhy8RkxLl2RaofTQ6Vzluwz0CId"      customer\_no: "2221000035830016"    }  ) {    cart {      customer\_no    }  } } |
| :---- |

**Response:**

| {     "data": {         "setCustomerNoOnCart": {             "cart": {                 "customer\_no": "2221000035830016"             }         }     } } |
| :---- |

46. # API set Gọi trước khi giao

**URL**: https://local.mmpro.vn/graphql  
**Method: POST**  
**Header:**

| Authorization : token\_customer login |
| :---- |

**Body:**

| mutation {  setCallBeforeDeliveryOnCart(    input: {      cart\_id: "cuFiSeqzdL3jk28mH88N2y9Qt0MGamB3"      is\_call\_before\_delivery: true    }  ) {    cart {      is\_call\_before\_delivery    }  } } |
| :---- |

**Response:**

| {     "data": {         "setCallBeforeDeliveryOnCart": {             "cart": {                 "is\_call\_before\_delivery": true             }         }     } } |
| :---- |

47. # API filter by created\_at or status in my account \- order page

URL: [https://local.mmpro.vn/graphql](https://local.mmpro.vn/graphql)  
Header: Authorization: Bearer \<customer\_token\>.  
Body: 

| query {   customer {     orders (       filter: {         createDateFrom: {gteq: "2024-01-01"}         createDateTo: { lteq: "2024-05-05"}         status: { eq: "picked\_ccod"}       }     ) {       items {         id         order\_date         number         status         customer\_no       }       total\_count       page\_info {         current\_page         page\_size         total\_pages       }     }   } }  |
| :---- |

Param description:

- createDateFrom: lọc bắt đầu từ ngày  
- createDateTo: lọc tới ngày  
- status: trạng thái của order

Response:

| {   "data": {     "customer": {       "orders": {         "items": \[           {             "id": "NDE0NzYw",             "order\_date": "2024-01-03 09:30:40",             "number": "26000048662",             "status": "Đã lấy hàng",             "customer\_no": null           },           {             "id": "NDQ2NTY2",             "order\_date": "2024-01-24 15:02:28",             "number": "26000053590",             "status": "Đã lấy hàng",             "customer\_no": null           },           {             "id": "NDQ2NTY3",             "order\_date": "2024-01-24 15:04:23",             "number": "26000053591",             "status": "Đã lấy hàng",             "customer\_no": null           },           {             "id": "NDU4MzQz",             "order\_date": "2024-02-02 08:20:21",             "number": "26000055622",             "status": "Đã lấy hàng",             "customer\_no": null           },           {             "id": "NDYzNjc1",             "order\_date": "2024-02-06 11:26:17",             "number": "26000056513",             "status": "Đã lấy hàng",             "customer\_no": null           },           {             "id": "NDY0Mzc4",             "order\_date": "2024-02-07 11:18:22",             "number": "26000056620",             "status": "Đã lấy hàng",             "customer\_no": null           },           {             "id": "NDk2OTYx",             "order\_date": "2024-03-09 10:16:21",             "number": "26000061857",             "status": "Đã lấy hàng",             "customer\_no": null           },           {             "id": "NTM3NDEy",             "order\_date": "2024-04-06 11:41:10",             "number": "26000067720",             "status": "Đã lấy hàng",             "customer\_no": null           },           {             "id": "NTczNzg3",             "order\_date": "2024-04-29 11:43:10",             "number": "26000073167",             "status": "Đã lấy hàng",             "customer\_no": null           }         \],         "total\_count": 9,         "page\_info": {           "current\_page": 1,           "page\_size": 20,           "total\_pages": 1         }       }     }   } } |
| :---- |

Response description:

48. # API available status my account \- order page

URL: [https://local.mmpro.vn/graphql](https://local.mmpro.vn/graphql)  
Body:

| query {     availableStatus {         status         label     } } |
| :---- |

Response:

|  |
| :---- |

49. # API Get List Store

+ Body

| Query query GetStoreLocators (    $store\_source\_type: Int,    $store\_city: Int    $store\_district: Int    $store\_ward: Int ) {  StoreLocators(    store\_source\_type: $store\_source\_type    store\_city: $store\_city    store\_district: $store\_district    store\_ward: $store\_ward  ) {    name    street    latitude    longitude    source\_image\_featured  } }  Variable {    "store\_source\_type": 1,    "store\_city": 724,    "store\_district": 18,    "store\_ward": 565 } |
| :---- |

+ Response:

| {    "data": {        "StoreLocators": \[            {                "name": "MM AN PHU",                "street": "Lot B, An Phu, An Khanh, An Phu Ward",                "latitude": null,                "longitude": null,                "source\_image\_featured": "https://mmprolocal.vn/media/inventory/source/m2-featured.png"            },            {                "name": "MM BINH PHU",                "street": "Binh Phu, Ward 11",                "latitude": null,                "longitude": null,                "source\_image\_featured": null            }        \]    } } |
| :---- |

4 giá trị Store\_source\_type, Store\_city, Store\_district, store\_ward được chọn trong Source detail, section Address Data .

- Store\_source\_type: Cột entity\_id trong bảng inventory\_source\_type  
- Store\_city: Cột region\_id trong bảng directory\_country\_region  
- Store\_district: Cột district\_code trong bảng mmvn\_temporary\_district\_import  
- store\_ward: Cột sub\_district\_code trong bảng directory\_country\_subdistrict

50. # API Online Payment Method

- **For MoMo & VNPAY**

1. **Set payment method**

**Syntax**

| mutation: {setPaymentMethodOnCart(input: SetPaymentMethodOnCartInput): SetPaymentMethodOnCartOutput}} |
| :---- |

**PaymentMethodInput attributes:**

| return\_url | URL used to redirect the page when the customer completes payment |
| :---- | :---- |

**Request:**

| mutation { setPaymentMethodOnCart(input: {   cart\_id: "IeTUiU0oCXjm0uRqGCOuhQ2AuQatogjG"   payment\_method: {     code: "momo\_wallet"     online\_payment: {       return\_url: "https://online.mmpro.vn/checkout-success"     }   } }) { cart {   selected\_payment\_method {     code   } }} |
| :---- |

**Response:** cart \- Cart\!

| { "data": {   "setPaymentMethodOnCart": {     "cart": {       "selected\_payment\_method": {         "code": "momo\_wallet"       }     }   } }} |
| :---- |

2. **Place Order**  
   

**Syntax**

| mutation: {placeOrder(input: PlaceOrderInput): PlaceOrderOutput}} |
| :---- |

**Request:**

| mutation {  placeOrder(input: { cart\_id: "IeTUiU0oCXjm0uRqGCOuhQ2AuQatogjG" }) {    orderV2 {      id      number      payment\_methods {        name        type        pay\_url      }    }    errors {      message      code    }  }} |
| :---- |

**Response:**

| {  "data": {    "placeOrder": {      "orderV2": {        "number": "000000006",        "payment\_methods": {          "name": "Thanh toán Ví MoMo",          "type": "momo\_wallet",          "pay\_url": "https://test-payment.momo.vn/v2/gateway/pay?t=TU9NT1Q1QloyMDIzMTIxM19URVNUfFBhcnRuZXJfVHJhbnNhY3Rpb25fSURfMTcyMTcyMDYyMDA3OA\&s=6c14385cd4355e0abe0e0563a2da20705bceca9fac79746b2bf6a4c380374b44"        }      },      "errors": \[\]    }  }} |
| :---- |

Get ***pay\_url*** in ***orderV2.payment\_methods.pay\_url*** to redirect to payment page

**Note:** Sau khi thanh toán xong sẽ được redirect về **return\_url** đã set ở **A.** Lấy params được trả về cùng để validate hoặc call API **C** để lấy trạng thái Order và redirect customer về trang (success or failure)

3. **Check payment result**  
   

**Syntax**

| query: {paymentResult(input: PaymentResultInput): PaymentResult}} |
| :---- |

**PaymentResult attributes:**

| response\_params | Parameters on return url after complete payment |
| :---- | :---- |

**Request:**

| query {  paymentResult(    input: {      response\_params: "?vnp\_Amount=4330000\&vnp\_BankCode=NCB\&vnp\_BankTranNo=VNP14665319\&vnp\_CardType=ATM\&vnp\_OrderInfo=000000020\&vnp\_PayDate=20241112013736\&vnp\_ResponseCode=00\&vnp\_TmnCode=MM000001\&vnp\_TransactionNo=14665319\&vnp\_TransactionStatus=00\&vnp\_TxnRef=000000020\&vnp\_SecureHash=fc1d522377046e35d815c0e0f935f7b28f24a479d420130710ea1c7e363f7b828721994aab6fdeb8e5b24e17b3b1f8941f4b64ba2d37115bb11b6e0212b2c2e8"    }  ) {    order\_id    status  }} |
| :---- |

**Response:**

| {  "data": {    "paymentResult": {      "order\_id": "000000020",      "status": true    }  }} |
| :---- |

51. # API trang liên hệ

## 50.1  Get config form liên hệ

**URL**: https://local.mmpro.vn/graphql  
**Method: POST**  
**Header:**  
**Body:**

| {  contactForm {    name    required    label    options {        value        label    }  } } |
| :---- |

**Response:**

| {     "data": {         "contactForm": \[             {                 "name": "name",                 "required": true,                 "label": "Họ tên",                 "options": \[\]             },             {                 "name": "email",                 "required": true,                 "label": "Email",                 "options": \[\]             },             {                 "name": "telephone",                 "required": true,                 "label": "Số điện thoại",                 "options": \[\]             },             {                 "name": "address",                 "required": true,                 "label": "Địa chỉ",                 "options": \[\]             },             {                 "name": "comment",                 "required": true,                 "label": "Nội dung cần hỗ trợ",                 "options": \[\]             }         \]     } } |
| :---- |

## 50.2  Gửi liên hệ

**URL**: https://local.mmpro.vn/graphql  
**Method: POST**  
**Header:**  
**Body:**

| type Mutation {    contactSubmit(        input: \[ContactFieldInput\]    ) } input ContactFieldInput {    field\_name: String    value: String } |
| :---- |

| mutation {    contactSubmit(        input: \[            {                field\_name: "name"                value: "Ta Huu Cuong"            },            {                field\_name: "email"                value: "cuongth@magenest.com"            },            {                field\_name: "telephone"                value: "0866466298"            },            {                field\_name: "address"                value: "Hoài Đức, Hà Nội"            },            {                field\_name: "comment"                value: "Tét comment"            },        \]    ) {        success        message    } } |
| :---- |

**Response:**

| {     "data": {         "contactSubmit": {             "success": true,             "message": "Cảm ơn bạn đã liên hệ với chúng tôi với các ý kiến và câu hỏi của bạn. Chúng tôi sẽ sớm trả lời bạn."         }     } } |
| :---- |

52. # API Get Source Type

+ Body

| Query query {  listSourceType {    id    name  } } Variable {} |
| :---- |

+ Response:

| {    "data": {        "listSourceType": \[            {                "id": 1,                "name": "Miền Bắc"            },            {                "id": 2,                "name": "Miền Nam"            },            {                "id": 3,                "name": "Miền Trung"            },            {                "id": 5,                "name": "Miền Tây"            }        \]    } } |
| :---- |

# 52\. API Get City by Source Type

+ Body

| Query query listCityBySourceType (    $source\_type: Int ) {  listCityBySourceType(    source\_type: $source\_type  ) {    province\_code    name  } } Variable {    "source\_type": 1 } |
| :---- |

+ Response:

| {    "data": {        "listCityBySourceType": \[            {                "province\_code": 1,                "name": "Thành phố Hà Nội"            },            {                "province\_code": 31,                "name": "Thành phố Hải Phòng"            }        \]    } } |
| :---- |

City được set trong Source type detail

- source\_type: Là Source type ID

# 53\. API Check tồn kho khi đổi store

+ Body

| query { checkStockByStore(     cart\_id: "vmj9v2aLaCczloAkYcW26XlDFya77BR3",     store\_code: "mm\_an\_phu" ) { store\_code store\_name items { sku name qty is\_in\_stock min\_qty max\_sale\_qty qty\_in\_cart manage\_stock sale\_qty is\_salable product {sku small\_image {url}}} } }  |
| :---- |

+ Response:

| {    "data": {        "checkStockByStore": {            "store\_code": "mm\_10010\_vi",            "store\_name": "AnPhuVN",            "items": \[                {                    "sku": "394521\_23945210",                    "name": "BAKING SODA 454G",                    "qty": 0,                    "is\_in\_stock": true,                    "min\_qty": 0,                    "max\_sale\_qty": 100000,                    "qty\_in\_cart": 1,                    "manage\_stock": false,                    "sale\_qty": 1000,                    "is\_salable": true,                    "product": {                        "sku": "394521\_23945210",                        "small\_image": {                            "url": "http://mmpro.local/media/catalog/product/placeholder/default/logommvn-320.jpg"                        }                    }                },                {                    "sku": "255752\_22557520",                    "name": "TUIZIPPER MMPRO XL 28\*35CM 50C",                    "qty": 0,                    "is\_in\_stock": true,                    "min\_qty": 0,                    "max\_sale\_qty": 100000,                    "qty\_in\_cart": 1,                    "manage\_stock": false,                    "sale\_qty": 1000,                    "is\_salable": true,                    "product": {                        "sku": "255752\_22557520",                        "small\_image": {                            "url": "http://mmpro.local/media/catalog/product/placeholder/default/logommvn-320.jpg"                        }                    }                },                {                    "sku": "82243\_20822439",                    "name": "BAOTAY WHITE GLOVES CPE SIZE M",                    "qty": 0,                    "is\_in\_stock": true,                    "min\_qty": 0,                    "max\_sale\_qty": 100000,                    "qty\_in\_cart": 4,                    "manage\_stock": false,                    "sale\_qty": 1000,                    "is\_salable": true,                    "product": {                        "sku": "82243\_20822439",                        "small\_image": {                            "url": "http://mmpro.local/media/catalog/product/placeholder/default/logommvn-320.jpg"                        }                    }                },                {                    "sku": "340432\_23404328",                    "name": "HOPNHUATRON MICRON 1450ML-9635",                    "qty": 0,                    "is\_in\_stock": true,                    "min\_qty": 0,                    "max\_sale\_qty": 100000,                    "qty\_in\_cart": 2,                    "manage\_stock": false,                    "sale\_qty": 1000,                    "is\_salable": true,                    "product": {                        "sku": "340432\_23404328",                        "small\_image": {                            "url": "http://mmpro.local/media/catalog/product/placeholder/default/logommvn-320.jpg"                        }                    }                },                {                    "sku": "169453\_21694530",                    "name": "HOPNHUA CHEFLOCK 1450ML 6115/1",                    "qty": 0,                    "is\_in\_stock": true,                    "min\_qty": 0,                    "max\_sale\_qty": 100000,                    "qty\_in\_cart": 1,                    "manage\_stock": false,                    "sale\_qty": 1000,                    "is\_salable": true,                    "product": {                        "sku": "169453\_21694530",                        "small\_image": {                            "url": "http://mmpro.local/media/catalog/product/placeholder/default/logommvn-320.jpg"                        }                    }                },                {                    "sku": "253722\_22537225",                    "name": "TAM TRE HP 1.8\*65MM \-1KG-HOP",                    "qty": 0,                    "is\_in\_stock": true,                    "min\_qty": 0,                    "max\_sale\_qty": 100000,                    "qty\_in\_cart": 1,                    "manage\_stock": false,                    "sale\_qty": 1000,                    "is\_salable": true,                    "product": {                        "sku": "253722\_22537225",                        "small\_image": {                            "url": "http://mmpro.local/media/catalog/product/placeholder/default/logommvn-320.jpg"                        }                    }                },                {                    "sku": "258726\_22587268",                    "name": "NRC HAPPY PRICE H.CHANH 20L",                    "qty": 0,                    "is\_in\_stock": true,                    "min\_qty": 0,                    "max\_sale\_qty": 100000,                    "qty\_in\_cart": 1,                    "manage\_stock": false,                    "sale\_qty": 1000,                    "is\_salable": true,                    "product": {                        "sku": "258726\_22587268",                        "small\_image": {                            "url": "http://mmpro.local/media/catalog/product/placeholder/default/logommvn-320.jpg"                        }                    }                },                {                    "sku": "321928\_23219281",                    "name": "BINH XIT NUOC 0.5L",                    "qty": 0,                    "is\_in\_stock": true,                    "min\_qty": 0,                    "max\_sale\_qty": 100000,                    "qty\_in\_cart": 4,                    "manage\_stock": false,                    "sale\_qty": 1000,                    "is\_salable": true,                    "product": {                        "sku": "321928\_23219281",                        "small\_image": {                            "url": "http://mmpro.local/media/catalog/product/placeholder/default/logommvn-320.jpg"                        }                    }                },                {                    "sku": "412666\_24126663",                    "name": "HOP KHAN GIAY CN SANO",                    "qty": 0,                    "is\_in\_stock": true,                    "min\_qty": 0,                    "max\_sale\_qty": 100000,                    "qty\_in\_cart": 2,                    "manage\_stock": false,                    "sale\_qty": 1000,                    "is\_salable": true,                    "product": {                        "sku": "412666\_24126663",                        "small\_image": {                            "url": "http://mmpro.local/media/catalog/product/placeholder/default/logommvn-320.jpg"                        }                    }                },                {                    "sku": "166949\_21669491",                    "name": "DAO GOT T.CAY TRON EAGLE11.5CM",                    "qty": 0,                    "is\_in\_stock": true,                    "min\_qty": 0,                    "max\_sale\_qty": 100000,                    "qty\_in\_cart": 2,                    "manage\_stock": false,                    "sale\_qty": 1000,                    "is\_salable": true,                    "product": {                        "sku": "166949\_21669491",                        "small\_image": {                            "url": "http://mmpro.local/media/catalog/product/placeholder/default/logommvn-320.jpg"                        }                    }                },                {                    "sku": "384304\_23843042",                    "name": "INT-TUI QUAI XACH 20\*30CM\*10KG",                    "qty": 6,                    "is\_in\_stock": true,                    "min\_qty": 0,                    "max\_sale\_qty": 100000,                    "qty\_in\_cart": 1,                    "manage\_stock": true,                    "sale\_qty": 5,                    "is\_salable": true,                    "product": {                        "sku": "384304\_23843042",                        "small\_image": {                            "url": "http://mmpro.local/media/catalog/product/placeholder/default/logommvn-320.jpg"                        }                    }                }            \]        }    } }  |
| :---- |

Còn hàng hay hết hàng is\_salable

# 54\. API kiểm tra thay đổi giá của sản phẩm trong cart

+ Body

| query { CheckPriceChange(    cart\_id: "EWIE18miVXywC23YIOTrHhlz2TxCYlxV" ) { is\_price\_change } }  |
| :---- |

+ Response:

| {    "data": {        "CheckPriceChange": {            "is\_price\_change": "1"        }    } }  |
| :---- |

# 55\. API giới hạn wishlist

Body

| query {   storeConfig {     wishlist\_limit   } } |
| :---- |

Response: 

| {   "data": {     "storeConfig": {       "wishlist\_limit": "10"     }   } } |
| :---- |

# API get danh sách gợi ý địa chỉ

**URL**: [https://local.mmpro.vn/graphql/](https://local.mmpro.vn/graphql/)  
**Body**:

| {   suggestLocation (       address: "91 Trung Hòa"   )   {        address       city       city\_code       district       district\_code       ward       ward\_code   } } |
| :---- |

**Response:**

| {     "data": {         "suggestLocation": \[             {                 "address": "91 Trung Hòa, Yên Hòa, Cầu Giấy, Hà Nội",                 "city": "Thành phố Hà Nội",                 "city\_code": "1",                 "district": "Quận Cầu Giấy",                 "district\_code": "5",                 "ward": "Phường Yên Hoà",                 "ward\_code": "172"             },             {                 "address": "91 Trung Hòa, Trung Hoa, Chương Mỹ, Hà Nội",                 "city": "Thành phố Hà Nội",                 "city\_code": "1",                 "district": "Huyện Chương Mỹ",                 "district\_code": "277",                 "ward": "Xã Đông Phương Yên",                 "ward\_code": "10030"             },             {                 "address": "Laika Cafe, 91 Trung Hòa, Yên Hòa, Cầu Giấy, Hà Nội",                 "city": "Thành phố Hà Nội",                 "city\_code": "1",                 "district": "Quận Cầu Giấy",                 "district\_code": "5",                 "ward": "Phường Yên Hoà",                 "ward\_code": "172"             },             {                 "address": "91 Trung Kính, Trung Hòa, Cầu Giấy, Hà Nội",                 "city": "Thành phố Hà Nội",                 "city\_code": "1",                 "district": "Quận Cầu Giấy",                 "district\_code": "5",                 "ward": "Phường Trung Hoà",                 "ward\_code": "175"             },             {                 "address": "91 P. Tú Mỡ, Trung Hoà, Cầu Giấy, Hà Nội",                 "city": "Thành phố Hà Nội",                 "city\_code": "1",                 "district": "Quận Cầu Giấy",                 "district\_code": "5",                 "ward": "Phường Trung Hoà",                 "ward\_code": "175"             }         \]     } } |
| :---- |

# 

53. # API get auto login MCard info

1. **Generate Login Info**

**Syntax**

| mutation generateLoginMcardInfo($input: GenerateLoginMcardInfoInput) {  generateLoginMcardInfo(input: $input) {    customer\_token    store\_view\_code  }} |
| :---- |

**Request Variables:**

| {  "input": {    "token": "MTkzOTcwMjY5NGMwMGpMSWRNSmI5aUoyd0t5U0IwNm90cm54VUJuMlRDTGRyUE9EdjRMSXFzWGk0ZUdNN0RpUmRqTXV2U0ZBcER5YVo=",    "store": "10010",    "cust\_no": "9999999999993",    "phone": "0999000098",    "cust\_no\_mm":"99999199999993",    "cust\_name":"haha"  }} |
| :---- |

**Response:**   
**TH1:** *customer\_token* not null \-\> Process login

| {  "data": {    "generateLoginMcardInfo": {      "customer\_token": "eyJraWQiOiIxIiwiYWxnIjoiSFMyNTYifQ.eyJ1aWQiOjIzNzM1LCJ1dHlwaWQiOjMsImlhdCI6MTczMjc2NzQ2NSwiZXhwIjoxNzMyNzcxMDY1fQ.emqGEL7-yG6unsX737wpOpMbK4fwO0DAMstsNUJuMfY",      "store\_view\_code": "b2c\_10010\_vi"    }  }} |
| :---- |

**TH2: Has an error** \-\> redirect to login page

| {  "errors": \[    {      "message": "Tài khoản chưa chính xác, vui lòng kiểm tra lại hoặc liên hệ CSKH",      "locations": \[        {          "line": 2,          "column": 3        }      \],      "path": \[        "generateLoginMcardInfo"      \],      "extensions": {        "category": "graphql-authentication"      }    }  \],  "data": {    "generateLoginMcardInfo": null  }} |
| :---- |

**TH3:** *customer\_token is null \-\>* process registration (see **B)**

| {  "data": {    "generateLoginMcardInfo": {      "customer\_token": null,      "store\_view\_code": "b2c\_10010\_vi"    }  }} |
| :---- |

2. **Create Customer**

**Syntax**

| mutation createCustomerFromMcard($input: CustomerCreateInput\!) {  createCustomerFromMcard(input: $input) {    customer\_token    customer {      email      firstname    }  }} |
| :---- |

**Request Variables:**

| {  "input": {    "email": "thaovt10@magenest.com",    "firstname": "thaovt1",    "lastname": "",    "is\_subscribed": false,    "custom\_attributes": \[      {        "attribute\_code": "company\_user\_phone\_number",        "value": "0000010"      },      {        "attribute\_code": "customer\_no",        "value": "0000001"      },      {        "attribute\_code": "mcard\_no",        "value": "0000001"      }    \]  }} |
| :---- |

**Response**

| {  "data": {    "createCustomerFromMcard": {      "customer\_token": "eyJraWQiOiIxIiwiYWxnIjoiSFMyNTYifQ.eyJ1aWQiOjExMTM0NywidXR5cGlkIjozLCJpYXQiOjE3MzM0MjEwOTAsImV4cCI6MTczMzQyNDY5MH0.-9YzmYonbvB554-Cl6DHAVtg0oyTDhQjuN1ZNldvjjU",      "customer": {        "email": "thaovt10@magenest.com",        "firstname": "thaovt1"      }    }  }} |
| :---- |

54. # API get Pdf List

    **URL**: https://local.mmpro.vn/graphql?query={listPdf(limit: 10\) {id title description url\_pdf url\_banner}}  
    **Method: GET**  
    **Param Description:**  
          **\-**  	limit: limit item trả về  
    **Response:**

| {     "data": {         "listPdf": \[             {                 "id": 51,                 "title": "B2B CATALOGUE 2025",                 "description": "",                 "url\_pdf": "https://local.mmpro.vn/media/pdf\_info/b/r/brd\_multiple\_store\_backoffice\_-\_v1.0.4-v123.pdf",                 "url\_banner": "https://local.mmpro.vn/media/Screenshot\_from\_2024-12-18\_17-10-24.png"             },             {                 "id": 52,                 "title": "B2B CATALOGUE 2024",                 "description": "",                 "url\_pdf": "https://local.mmpro.vn/media/pdf\_info/c/a/catalogue\_mmpro\_eng.pdf",                 "url\_banner": "https://local.mmpro.vn/media/b2b-catalogue-2024\_1.jpg"             }         \]     } } |
| :---- |

55. # API get blog

Url: [https://local.mmpro.vn/graphql](https://local.mmpro.vn/graphql)  
Method: GET  
Query: 

| query {    blogList(        sort: {            field: "publish\_date",            direction: "DESC"        },        currentPage: 1,        pageSize: 2    ) {        items {            id            title            url\_key            content            short\_content            image            source\_edition            image\_thumb            tags            meta\_title            meta\_keywords            meta\_description            publish\_date            is\_active            views        }        page\_info {            current\_page            page\_size            total\_pages        }    } } |
| :---- |

Param description:

- sort: xác định cách sort.  
  - field: attribute để sort, default là views. Có thể dùng publish\_date để sort theo ngày được publish của blog.  
  - direction: chiều sort (ASC, DESC) default là DESC.

Response

| {    "data": {        "blogList": {            "items": \[                {                    "id": "23",                    "title": "B2B WORKSHOP \- UNILEVER FOOD SOLUTION \- HO CHI MINH TEAM",                    "url\_key": "b2b-workshop-unilever-food-solution-ho-chi-minh-team",                    "content": "\<p\>\<img src=\\"https://mmpro.vn/media/Slide2\_3.PNG\\" alt=\\"\\" width=\\"1800\\"\>\</p\>",                    "short\_content": "\<p\>Hơn 30 doanh nghiệp trong thuộc các trung tâm tổ chức sự kiện khu vực Hồ Chí Minh đã cùng nhau tham dự workshop \\"Ý tưởng mùa tiệc cưới\\" do Unilever Food Solutions và MM Mega Market tổ chức.\</p\>\\r\\n\<p\>\<em\>More than 30 businesses from event organizing centers in the Ho Chi Minh City area gathered to participate in the workshop \\"Wedding Season Ideas,\\" hosted by Unilever Food Solutions and MM Mega Market.\</em\>\</p\>",                    "image": "https://local.mmpro247.vn/media/magearray/news/image/Slide1\_3.PNG",                    "source\_edition": "B2B DEVELOPMENT",                    "image\_thumb": "",                    "tags": "MM Mega Market, Unilever Food Solutions",                    "meta\_title": "",                    "meta\_keywords": "",                    "meta\_description": "",                    "publish\_date": "2024-11-16",                    "is\_active": true,                    "views": 1                },                {                    "id": "22",                    "title": "B2B TRAINING \- WILMAR \- HO CHI MINH TEAM",                    "url\_key": "b2b-training-wilmar-ho-chi-minh-team",                    "content": "\<p\>\<img src=\\"https://mmpro.vn/media/Slide2\_2.PNG\\" alt=\\"\\" width=\\"1800\\"\>\</p\>",                    "short\_content": "\<p\>Chương trình đào tạo chuyên sâu dành\&nbsp;cho đội ngũ bán hàng MM Mega Market của Wilmar với sự góp mặt của siêu đầu bếp Lê Xuân Tâm\&nbsp;đã trở thành một trong những bước tiến quan trọng trong mối quan hệ giữa hai doanh nghiệp.\</p\>\\r\\n\<div class=\\"group/conversation-turn relative flex w-full min-w-0 flex-col agent-turn\\"\>\\r\\n\<div class=\\"flex-col gap-1 md:gap-3\\"\>\\r\\n\<div class=\\"flex max-w-full flex-col flex-grow\\"\>\\r\\n\<div class=\\"min-h-8 text-message flex w-full flex-col items-end gap-2 whitespace-normal break-words \[.text-message+\&amp;\]:mt-5\\" dir=\\"auto\\" data-message-author-role=\\"assistant\\" data-message-id=\\"47296c47-51e4-47ca-a651-d6c24e0610df\\" data-message-model-slug=\\"gpt-4o-mini\\"\>\\r\\n\<div class=\\"flex w-full flex-col gap-1 empty:hidden first:pt-\[3px\]\\"\>\\r\\n\<div class=\\"markdown prose w-full break-words dark:prose-invert light\\"\>\\r\\n\<p\>\<em\>The specialized training program for the MM Mega Market sales team, organized by Wilmar and featuring the renowned Chef Le Xuan Tam, has become one of the significant milestones in the relationship between the two companies.\</em\>\</p\>\\r\\n\</div\>\\r\\n\</div\>\\r\\n\</div\>\\r\\n\</div\>\\r\\n\</div\>\\r\\n\</div\>",                    "image": "https://local.mmpro247.vn/media/magearray/news/image/Slide1\_2.PNG",                    "source\_edition": "B2B DEVELOPMENT",                    "image\_thumb": "",                    "tags": "MM Mega Market, Wilmar",                    "meta\_title": "",                    "meta\_keywords": "",                    "meta\_description": "",                    "publish\_date": "2024-11-15",                    "is\_active": true,                    "views": 3                }            \],            "page\_info": {                "current\_page": 1,                "page\_size": 2,                "total\_pages": 9            }        }    } }  |
| :---- |

56. # API get blog details

URL: [https://local.mmpro247.vn/graphql](https://local.mmpro247.vn/graphql)  
Method: GET  
Query

| query {    blogList(        urlKey: "b2b-workshop-unilever-food-solution-ho-chi-minh-team"    ) {        items {            id            title            url\_key            content            short\_content            image            source\_edition            image\_thumb            tags            meta\_title            meta\_keywords            meta\_description            publish\_date            is\_active            views        }    } } |
| :---- |

Param description:

- urlKey: urlKey của blog

Response

| {    "data": {        "blogList": {            "items": \[                {                    "id": "23",                    "title": "B2B WORKSHOP \- UNILEVER FOOD SOLUTION \- HO CHI MINH TEAM",                    "url\_key": "b2b-workshop-unilever-food-solution-ho-chi-minh-team",                    "content": "\<p\>\<img src=\\"https://mmpro.vn/media/Slide2\_3.PNG\\" alt=\\"\\" width=\\"1800\\"\>\</p\>",                    "short\_content": "\<p\>Hơn 30 doanh nghiệp trong thuộc các trung tâm tổ chức sự kiện khu vực Hồ Chí Minh đã cùng nhau tham dự workshop \\"Ý tưởng mùa tiệc cưới\\" do Unilever Food Solutions và MM Mega Market tổ chức.\</p\>\\r\\n\<p\>\<em\>More than 30 businesses from event organizing centers in the Ho Chi Minh City area gathered to participate in the workshop \\"Wedding Season Ideas,\\" hosted by Unilever Food Solutions and MM Mega Market.\</em\>\</p\>",                    "image": "https://local.mmpro247.vn/media/magearray/news/image/Slide1\_3.PNG",                    "source\_edition": "B2B DEVELOPMENT",                    "image\_thumb": "",                    "tags": "MM Mega Market, Unilever Food Solutions",                    "meta\_title": "",                    "meta\_keywords": "",                    "meta\_description": "",                    "publish\_date": "2024-11-16",                    "is\_active": true,                    "views": 1                }            \]        }    } } |
| :---- |

57. # API get blog category list

Method: GET  
Query: 

| query {    blogCategory {        categories {            id            name            url\_key            meta\_title            meta\_keywords            meta\_description            blog\_count        }    } } |
| :---- |

Response: 

| {    "data": {        "blogCategory": {            "categories": \[                {                    "id": "1",                    "name": "Tin tức",                    "url\_key": "tin-tuc",                    "meta\_title": "Mega Market Tin Tức Mới Nhất",                    "meta\_keywords": "MegaMarket, Tin Tức, News",                    "meta\_description": "Mega Market Tin Tức Mới Nhất",                    "blog\_count": 1                },                {                    "id": "2",                    "name": "News",                    "url\_key": "news",                    "meta\_title": "News",                    "meta\_keywords": "MegaMarket, Tin Tức, News",                    "meta\_description": "Mega Market News daily",                    "blog\_count": 2                },                {                    "id": "3",                    "name": "B2B BLOG",                    "url\_key": "b2b-blog",                    "meta\_title": "B2B BLOG \- MM Pro Mới Nhất",                    "meta\_keywords": "",                    "meta\_description": "B2B BLOG \- MM Pro cung cấp cho khách hàng doanh nghiệp đa dạng sản phẩm với nhiều ưu đãi. Giao hàng tận nơi. Miễn phí vận chuyển. Chất lượng đảm bảo. Chính hãng",                    "blog\_count": 11                },                {                    "id": "4",                    "name": "Hệ thống trung tâm MM Mega Market Việt Nam",                    "url\_key": "stores-mm-pro",                    "meta\_title": "Hệ thống trung tâm MM Mega Market Việt Nam | MM Pro",                    "meta\_keywords": "",                    "meta\_description": "MM Pro với hệ thống trung tâm MM Mega Market Việt Nam ở Bắc, Trung Nam gồm: Hà Nội, TP. HCM, Hải Phòng, Đà Nẵng, Quy Nhơn, Bình Dương, Vũng Tàu, Cần Thơ, Long Xuyên,...",                    "blog\_count": 3                }            \]        }    } } |
| :---- |

58. # API search blog

Method: GET  
Query List Blog In Category

| query {    searchNews(        search: "",        date: ""        categoryId: "3",        currentPage: 1,        pageSize: 2    ) {        items {            id            title            url\_key            content            short\_content            image            source\_edition            image\_thumb            tags            meta\_title            meta\_keywords            meta\_description            publish\_date            is\_active            views        }        page\_info {            current\_page            page\_size            total\_pages        }    } } |
| :---- |

Param description:

- search: input text

Response

| {    "data": {        "searchNews": {            "items": \[                {                    "id": "15",                    "title": "MM MEGA MARKET ĐỒNG HÀNH CÙNG BOCUSE D'OR VIỆT NAM 2024",                    "url\_key": "mm-mega-market-ng-h-nh-c-ng-bocuse-d-or-vi-t-nam-2024",                    "content": "\<p\>\<img src=\\"https://mmpro.vn/media/Bocuse\_E-News-03\_1.png\\" alt=\\"\\" width=\\"1800\\"\>\</p\>",                    "short\_content": "\<p\>Đánh dấu 10 năm quay trở lại kể từ lần hợp tác vào năm 2014, MM Mega Market vinh dự đồng hành cùng Đội Đầu bếp Bocuse Việt Nam,hướng dẫn bởi Đầu bếp Sakal Phoeung \- Chủ tịch Bocuse d'Or Việt Nam, để tranh tài tại Bocuse d’Or 2024.\</p\>\\r\\n\<p\>\&nbsp;\</p\>",                    "image": "https://mmpro.izysync.com/media/magearray/news/image/bocuse-e-news-3-01\_optimized.1.png",                    "source\_edition": "MM MEGA MARKET \- B2B DEVELOPMENT",                    "image\_thumb": "",                    "tags": "MM PROFESSIONAL, WE ARE FRESH, BOCUSE D'OR VIETNAM TEAM",                    "meta\_title": "",                    "meta\_keywords": "",                    "meta\_description": "",                    "publish\_date": "2024-08-30",                    "is\_active": true,                    "views": 49                },                {                    "id": "8",                    "title": "ESCOFFIER WORKSHOP \- THE APPLICATION OF SALMON",                    "url\_key": "escoffier-workshop-the-application-of-salmon",                    "content": "\<p\>\<img src=\\"https://mmpro.vn/media/Workshop\_Escoffier\_20.6.2024.CONTENT1.jpg\\" alt=\\"\\"\>\</p\>",                    "short\_content": "\<p\>Cá Hồi Tươi Nauy được nhập khẩu và phân phối trực tiếp trên toàn hệ thống MM cả nước. Chúng tôi tự hào là một trong những lựa chọn hàng đầu của các đầu bếp uy tín trong nền ẩm thực.\</p\>\\r\\n\<p\>\<em\>Norwegian Fresh Salmon is imported and distributed directly throughout the MM store nationwide. We are proud to be one of the top choices of prestigious chefs in the culinary industry.\</em\>\</p\>",                    "image": "https://mmpro.izysync.com/media/magearray/news/image/banner-02.jpg",                    "source\_edition": "MM MEGA MARKET \- B2B DEVELOPMENT",                    "image\_thumb": "",                    "tags": "MM DIRECT IMPORT, NORWAY SALMON, CHILLED SALMON, WE ARE FRESH",                    "meta\_title": "",                    "meta\_keywords": "",                    "meta\_description": "",                    "publish\_date": "2024-06-20",                    "is\_active": true,                    "views": 268                }            \],            "page\_info": {                "current\_page": 1,                "page\_size": 2,                "total\_pages": 4            }        }    } }  |
| :---- |

Query get List In Archived Blog

| query {    searchNews(        search: "",        date: "2024-08"        categoryId: "",        currentPage: 1,        pageSize: 2    ) {        items {            id            title            url\_key            content            short\_content            image            source\_edition            image\_thumb            tags            meta\_title            meta\_keywords            meta\_description            publish\_date            is\_active            views        }        page\_info {            current\_page            page\_size            total\_pages        }    } } |
| :---- |

Response: 

| {    "data": {        "searchNews": {            "items": \[                {                    "id": "15",                    "title": "MM MEGA MARKET ĐỒNG HÀNH CÙNG BOCUSE D'OR VIỆT NAM 2024",                    "url\_key": "mm-mega-market-ng-h-nh-c-ng-bocuse-d-or-vi-t-nam-2024",                    "content": "\<p\>\<img src=\\"https://mmpro.vn/media/Bocuse\_E-News-03\_1.png\\" alt=\\"\\" width=\\"1800\\"\>\</p\>",                    "short\_content": "\<p\>Đánh dấu 10 năm quay trở lại kể từ lần hợp tác vào năm 2014, MM Mega Market vinh dự đồng hành cùng Đội Đầu bếp Bocuse Việt Nam,hướng dẫn bởi Đầu bếp Sakal Phoeung \- Chủ tịch Bocuse d'Or Việt Nam, để tranh tài tại Bocuse d’Or 2024.\</p\>\\r\\n\<p\>\&nbsp;\</p\>",                    "image": "https://mmpro.izysync.com/media/magearray/news/image/bocuse-e-news-3-01\_optimized.1.png",                    "source\_edition": "MM MEGA MARKET \- B2B DEVELOPMENT",                    "image\_thumb": "",                    "tags": "MM PROFESSIONAL, WE ARE FRESH, BOCUSE D'OR VIETNAM TEAM",                    "meta\_title": "",                    "meta\_keywords": "",                    "meta\_description": "",                    "publish\_date": "2024-08-30",                    "is\_active": true,                    "views": 49                }            \],            "page\_info": {                "current\_page": 1,                "page\_size": 2,                "total\_pages": 1            }        }    } } |
| :---- |

Query get List Blog By Search Term

| query {    searchNews(        search: "test",        date: ""        categoryId: "",        currentPage: 1,        pageSize: 2    ) {        items {            id            title            url\_key            content            short\_content            image            source\_edition            image\_thumb            tags            meta\_title            meta\_keywords            meta\_description            publish\_date            is\_active            views        }        page\_info {            current\_page            page\_size            total\_pages        }    } }  |
| :---- |

Response:

| {    "data": {        "searchNews": {            "items": \[                {                    "id": "19",                    "title": "Test Post",                    "url\_key": "test-post",                    "content": "\<p\>hnay trời thật đẹp\!\!\</p\>",                    "short\_content": "\<p\>Test post 17/11\</p\>",                    "image": "https://mmpro.izysync.com/media/magearray/news/image/BANNER-MAIL-2424---580x470\_1.webp",                    "source\_edition": null,                    "image\_thumb": "",                    "tags": "",                    "meta\_title": "",                    "meta\_keywords": "",                    "meta\_description": "",                    "publish\_date": "2024-11-17",                    "is\_active": true,                    "views": 0                },                {                    "id": "18",                    "title": "Bài Post",                    "url\_key": "b-i-post",                    "content": "Test123",                    "short\_content": "\<p\>Hệ thống MMVN là hệ thống...\</p\>",                    "image": "https://mmpro.izysync.com/media/magearray/news/image/sua-bot-abbott-grow-3-900g\_3.webp",                    "source\_edition": null,                    "image\_thumb": "",                    "tags": "",                    "meta\_title": "",                    "meta\_keywords": "",                    "meta\_description": "",                    "publish\_date": "2024-11-14",                    "is\_active": true,                    "views": 2                }            \],            "page\_info": {                "current\_page": 1,                "page\_size": 2,                "total\_pages": 1            }        }    } } |
| :---- |

59. # API get archived blog list

Method: GET  
Query

| query {    archivedBlog {        archived\_blogs {            name            date            blog\_count        }    } } |
| :---- |

Response

| {    "data": {        "archivedBlog": {            "archived\_blogs": \[                {                    "name": "November 2024",                    "date": "2024-11",                    "blog\_count": 3                },                {                    "name": "October 2024",                    "date": "2024-10",                    "blog\_count": 4                },                {                    "name": "September 2024",                    "date": "2024-09",                    "blog\_count": 6                },                {                    "name": "August 2024",                    "date": "2024-08",                    "blog\_count": 7                },                {                    "name": "July 2024",                    "date": "2024-07",                    "blog\_count": 10                },                {                    "name": "June 2024",                    "date": "2024-06",                    "blog\_count": 11                },                {                    "name": "May 2024",                    "date": "2024-05",                    "blog\_count": 12                },                {                    "name": "April 2024",                    "date": "2024-04",                    "blog\_count": 15                },                {                    "name": "May 2023",                    "date": "2023-05",                    "blog\_count": 16                }            \]        }    } } |
| :---- |

60. # API increase view when click to blog 

Body: 

| mutation {    increaseBlogView(input: {urlKey: "mega-market-ra-mat-sieu-thi-hien-dai-gia-tot"}) {        has\_increase    } } |
| :---- |

Response:

| {    "data": {        "increaseBlogView": {            "has\_increase": true        }    } } |
| :---- |

61. # Blog breadcrumb

FE fix cứng phần breadcrumb theo format sau:

- Ở trang blog details: gắn blog title vào sau. (Home \> News \> Blog Title)  
- Ở trang category blog list page, gắn category name vào sau. (Home \> News \> Category Name)  
- Ở trang archived blog list blog page, gắn tên của archived blog vào sau (Home \> News \> November 2024\)  
- Ở trang kết quả tìm kiếm, gắn text theo dạng: Kết quả tìm kiếm cho "test" 

62. # API LoginAsCustomer \- GenerateCustomerTokenBySecret

URL: https://b2c.mmpro.local/loginascustomer/login/index/secret/4CmYmcpZbNG5xYAZwfFkGmxpYxwYHXlDDeEj95iMnRSRMEMpabD1pODo55RpuaSD/

FE get secret key từ url để call API:

**Syntax**

| mutation generateCustomerTokenBySecret ($input: GenerateCustomerTokenBySecretInput\!) {    generateCustomerTokenBySecret (input: $input) {        customer\_token    }} |
| :---- |

**Request Variables:**

| {  "input": {    "secret": "4CmYmcpZbNG5xYAZwfFkGmxpYxwYHXlDDeEj95iMnRSRMEMpabD1pODo55RpuaSD"  }} |
| :---- |

**Response:**   
**TH1:** *customer\_token* not null \-\> Process login

| {  "data": {    "generateCustomerTokenBySecret": {      "customer\_token": "eyJraWQiOiIxIiwiYWxnIjoiSFMyNTYifQ.eyJ1aWQiOjIzNzM1LCJ1dHlwaWQiOjMsImlhdCI6MTczODcyNDIxNSwiZXhwIjoxNzM4NzI3ODE1fQ.qyX4BNQyEkj0XeC-5ITNYmfC2I\_v\_fYFXG3rUcloMEA"    }  }} |
| :---- |

**Error:**

| {  "errors": \[    {      "message": "Cannot login to account. Secret key is not valid.",      "locations": \[        {          "line": 2,          "column": 5        }      \],      "path": \[        "generateCustomerTokenBySecret"      \],      "extensions": {        "category": "graphql-authentication"      }    }  \],  "data": {    "generateCustomerTokenBySecret": null  }} |
| :---- |

63. # API Check remove Item Not visibility In cart

URL: [https://local.mmpro.vn/graphql](https://local.mmpro.vn/graphql)  
Header: Authorization: Bearer \<token\_customer\>

| {    removeItemNotVisibleFromCart(        cart\_id: "P7rtS4UYDjRHaHMRIevGl1FWbAYOfEpg"    ) {        is\_removed        messages    } } |
| :---- |

Response

| {    "data": {        "removeItemNotVisibleFromCart": {            "is\_removed": true,            "messages": "Các sản phẩm 108859 không có hàng tại khu vực bạn vừa chọn và đã được bỏ ra khỏi giỏ hàng."        }    } } |
| :---- |

64. # API bổ sung order totals

Body:

| query GetCustomerOrders($filter:CustomerOrdersFilterInput $pageSize:Int\!) {    customer{        orders(            filter: $filter            pageSize: $pageSize        ) {            items {                total {                    base\_total\_after\_discount {                        currency                        value                    }                }            }        }    } } |
| :---- |

Response:

| {    "data": {        "customer": {            "orders": {                "items": \[                    {                        "total": {                            "base\_total\_after\_discount": {                                "currency": "VND",                                "value": 593000                            }                        }                    }                \]            }        }    } } |
| :---- |

# API MCARD

## **Mcard OTP GraphQL API Documentation**

✅ Common Information

* **Endpoint:** `/graphql`

* **Header:** `Content-Type: application/json`

* **Method:** `POST`

---

🔹 Mutation: `McardSendOtp`

Gửi mã OTP tới số điện thoại

`mutation {`  
  `McardSendOtp(phone: "0975164296", resend: 0) {`  
    `status`  
    `message`  
  `}`  
`}`

Parameters

| Name | Type | Required | Description |
| ----- | ----- | ----- | ----- |
| phone | String | Yes | Số điện thoại cần gửi OTP |
| resend | Int | Yes | 0 \= gửi mới, 1 \= gửi lại OTP |

Response

`{`  
  `"data": {`  
    `"McardSendOtp": {`  
      `"status": 1,`  
      `"message": "Please open your Mcard app to receive OTP code."`  
    `}`  
  `}`  
`}`

---

🔹 Mutation: `McardVerifyOtp`

Xác thực mã OTP

`mutation {`  
  `McardVerifyOtp(phone: "0975164296", otp: "123456") {`  
    `status`  
    `token`  
    `has_existed`  
    `customer_token`  
    `message`  
  `}`  
`}`

Parameters

| Name | Type | Required | Description |
| ----- | ----- | ----- | ----- |
| phone | String | Yes | Số điện thoại |
| otp | String | Yes | Mã OTP được gửi từ Mcard |

Response

`{`  
  `"data": {`  
    `"McardVerifyOtp": {`  
      `"status": 1,`  
      `"token": "mcard_token_here",`  
      `"has_existed": 1,`  
      `"customer_token": "magento_customer_token",`  
      `"message": "Success"`  
    `}`  
  `}`  
`}`

* `has_existed`: `1` nếu customer đã tồn tại trong Magento, `0` nếu chưa.

* `customer_token`: Token đăng nhập Magento (nếu đã tồn tại).

---

🔹 Mutation: `McardRegisterByToken`

Đăng ký tài khoản Magento từ thông tin người dùng Mcard

`mutation {`  
  `McardRegisterByToken(`  
    `email: "thienpv@email.com",`  
    `phone: "0975164296",`  
    `token: "mcard_user_session_token"`  
  `) {`  
    `status`  
    `message`  
    `customer_token`  
    `token`  
  `}`  
`}`

Parameters

| Name | Type | Required | Description |
| ----- | ----- | ----- | ----- |
| email | String | Yes | Email để đăng ký Magento |
| phone | String | Yes | Số điện thoại để kiểm tra Mcard info |
| token | String | Yes | Token từ API verify OTP của Mcard |

Response

`{`  
  `"data": {`  
    `"McardRegisterByToken": {`  
      `"status": 1,`  
      `"message": "Customer created successfully",`  
      `"customer_token": "magento_customer_token",`  
      `"token": "mcard_user_token"`  
    `}`  
  `}`  
`}`

* `customer_token`: Token đăng nhập Magento (nếu đăng ký thành công).

## **Mcard E-voucher GraphQL API Documentation**

✅ Common Information

* **Endpoint:** `/graphql`

* **Header:** `Content-Type: application/json`

* **Method:** `GET`

###  Mutation: `getEvoucherCustomer`

Lấy voucher available từ customer\_no

http://mmpro.local/graphql?query=query%20%7B%20getEvoucherCustomer(customer\_no%3A%20%222225000058348%22%2C%20page%3A%201%2C%20limit%3A%2020)%20%7B%20status%20message%20total%20gifts%20%7B%20evoucher\_id%20evoucher\_name%20thumbnail%20code%20status%20amount%20expired\_at%20%7D%20%7D%20%7D

`query { getEvoucherCustomer(customer_no: "2220001722216", page: 1, limit: 20) {`  
    `status`  
    `message`  
    `total`  
    `gifts {`  
      `evoucher_id`  
      `evoucher_name`  
      `thumbnail`  
      `code`  
      `status`  
      `amount`  
      `expired_at`  
    `}`  
`}`  
`}`

Parameters

| Name | Type | Required | Description |
| ----- | ----- | ----- | ----- |
| customer\_no | String | Yes | Customer no |
| page | Int | Yes | Page ( bắt đầu từ 1 ) |
| limit | String | Yes | Limit (nhỏ hơn 20\) |

Response

`{`  
    `"data": {`  
        `"getEvoucherCustomer": {`  
            `"status": 1,`  
            `"message": "Successsfully",`  
            `"total": "2",`  
            `"gifts": [`  
                `{`  
                    `"evoucher_id": "64df3662135f6195aca873c5",`  
                    `"evoucher_name": "VOUCHER 50.000Ð",`  
                    `"thumbnail": "https://mcdn.mmvietnam.app/gifts/mm/mmvc_50k.jpg",`  
                    `"code": "4002440953812",`  
                    `"status": "1",`  
                    `"amount": "50000",`  
                    `"expired_at": "2023-09-01 16:59:59"`  
                `},`  
                `{`  
                    `"evoucher_id": "64deefee656da48037fa1ecc",`  
                    `"evoucher_name": "VOUCHER 50.000Ð",`  
                    `"thumbnail": "https://mcdn.mmvietnam.app/gifts/mm/mmvc_50k.jpg",`  
                    `"code": "7711254717219",`  
                    `"status": "1",`  
                    `"amount": "50000",`  
                    `"expired_at": "2023-09-01 16:59:59"`  
                `}`  
            `]`  
        `}`  
    `}`  
`}`

* `status`: 1 thành công, 0 thất bại  
* `status` ( gift ) :   
  1 STATUS\_ACTIVE,   
  2 STATUS\_EXPIRED,   
  3 STATUS\_USED,  
  4 STATUS\_REDEEMED

### Mutation: `ApplyGiftCardToCart`

Áp dụng mã Gift Card vào giỏ hàng hiện tại của khách hàng.

URL

`POST /graphql`

Headers

`Content-Type: application/json`

`Authorization: Bearer <customer-token>`

Mutation

`mutation ApplyGiftCardToCart($cartId: String!, $giftCardCode: String!) {`

  `applyAmGiftCardToCart(`

    `input: {`

      `cart_id: $cartId`

      `am_gift_card_code: $giftCardCode`

    `}`

  `) {`

    `cart {`

      `applied_am_gift_cards {`

        `code`

        `expiration_date`

        `current_balance {`

          `value`

          `currency`

        `}`

        `applied_balance {`

          `value`

          `currency`

        `}`

      `}`

    `}`

  `}`

`}`

Variables

`{`

  `"cartId": "v1hmsdf23KLJ09sdjdfK",`  

  `"giftCardCode": "GIFTCARD2024"`

`}`

Response

`{`

  `"data": {`

    `"applyAmGiftCardToCart": {`

      `"cart": {`

        `"applied_am_gift_cards": [`

          `{`

            `"code": "GIFTCARD2024",`

            `"expiration_date": "2025-12-31",`

            `"current_balance": {`

              `"value": 50,`

              `"currency": "USD"`

            `},`

            `"applied_balance": {`

              `"value": 20,`

              `"currency": "USD"`

            `}`

          `}`

        `]`

      `}`

    `}`

  `}`

`}`

Giải thích

| Trường | Mô tả |
| ----- | ----- |
| `cart_id` | ID giỏ hàng của khách hàng (có thể lấy từ `createEmptyCart`) |
| `am_gift_card_code` | Mã gift card cần áp dụng |
| `applied_am_gift_cards` | Danh sách các mã gift card đã được áp dụng vào giỏ hàng |
| `current_balance` | Số dư còn lại của mã gift card |
| `applied_balance` | Số tiền đã được áp dụng vào giỏ hàng |

### Mutation: `removeAmGiftCardFromCart`

Xóa mã Gift Card đã áp dụng ra khỏi giỏ hàng hiện tại của khách hàng.

URL

`POST /graphql`

Headers

`Content-Type: application/json`

`Authorization: Bearer <customer-token>`

Mutation

graphql

`mutation RemoveGiftCardFromCart($cartId: String!, $giftCardCode: String!) {`

  `removeAmGiftCardFromCart(`

    `input: {`

      `cart_id: $cartId`

      `am_gift_card_code: $giftCardCode`

    `}`

  `) {`

    `cart {`

      `applied_am_gift_cards {`

        `code`

        `expiration_date`

        `current_balance {`

          `value`

          `currency`

        `}`

        `applied_balance {`

          `value`

          `currency`

        `}`

      `}`

    `}`

  `}`

`}`

Variables

json

`{`

  `"cartId": "v1hmsdf23KLJ09sdjdfK",`

  `"giftCardCode": "GIFTCARD2024"`

`}`

Response

`{`

  `"data": {`

    `"removeAmGiftCardFromCart": {`

      `"cart": {`

        `"applied_am_gift_cards": []`

      `}`

    `}`

  `}`

`}`

Giải thích

| Trường | Mô tả |
| ----- | ----- |
| `cart_id` | ID giỏ hàng của khách hàng |
| `am_gift_card_code` | Mã gift card cần xóa khỏi giỏ |
| `applied_am_gift_cards` | Danh sách các gift card còn lại sau khi xóa (rỗng nếu đã xóa hết) |
| `current_balance` | Số dư còn lại của mã gift card |
| `applied_balance` | Số tiền đã được áp dụng vào giỏ hàng trước đó |

### Query: `cart.applied_am_gift_cards`

Truy xuất các mã gift card đã được áp dụng vào giỏ hàng hiện tại.

Thêm applied\_am\_gift\_cards vào query get cart

Query

`query GetAppliedGiftCards($cartId: String!) {`

  `cart(cart_id: $cartId) {`

    `applied_am_gift_cards {`

      `code`

      `expiration_date`

      `current_balance {`

        `value`

        `currency`

      `}`

      `applied_balance {`

        `value`

        `currency`

      `}`

    `}`

  `}`

`}`

Ví dụ kết quả

`{`

  `"data": {`

    `"cart": {`

      `"applied_am_gift_cards": [`

        `{`

          `"code": "GIFTCARD2024",`

          `"expiration_date": "2025-12-31",`

          `"current_balance": {`

            `"value": 30,`

            `"currency": "USD"`

          `},`

          `"applied_balance": {`

            `"value": 20,`

            `"currency": "USD"`

          `}`

        `}`

      `]`

    `}`

  `}`

`}`

### **`amUserGiftCardAccount: [AmGiftCardAccount]`**

Query này sẽ trả về **danh sách các gift card (voucher) thuộc về tài khoản khách hàng đã đăng nhập**, bao gồm cả những mã đã sử dụng hoặc còn hiệu lực.

---

 Query: `amUserGiftCardAccount`

URL

`POST /graphql`

Headers

`Content-Type: application/json`  

`Authorization: Bearer <customer-token>`

Query

`query {`

  `amUserGiftCardAccount {`

    `code`

    `expiration_date`

    `status`

    `current_balance {`

      `value`

      `currency`

    `}`

    `usage`

  `}`

`}`

Ví dụ kết quả

`{`

  `"data": {`

    `"amUserGiftCardAccount": [`

      `{`

        `"code": "GIFTCARD2023",`

        `"expiration_date": "2025-01-01",`

        `"status": "USED",`

        `"current_balance": {`

          `"value": 0,`

          `"currency": "USD"`

        `},`

        `"usage": "Order #100001234"`

      `},`

      `{`

        `"code": "GIFTCARD2024",`

        `"expiration_date": "2025-12-31",`

        `"status": "ACTIVE",`

        `"current_balance": {`

          `"value": 50,`

          `"currency": "USD"`

        `},`

        `"usage": "Not used"`

      `}`

    `]`

  `}`

`}`

---

ℹ️ Các field hữu ích

| Field | Ý nghĩa |
| ----- | ----- |
| `code` | Mã gift card |
| `expiration_date` | Ngày hết hạn |
| `status` | Trạng thái (ACTIVE, USED, EXPIRED, etc.) |
| `current_balance` | Số dư còn lại |
| `usage` | Ghi chú đơn hàng hoặc trạng thái sử dụng |

[image1]: <data:image/png;base64,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>

[image2]: <data:image/png;base64,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>

[image3]: <data:image/png;base64,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>