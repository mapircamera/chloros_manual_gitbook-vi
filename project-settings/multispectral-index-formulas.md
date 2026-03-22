---
description: This page lists some multispectral indices that Chloros uses
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/o044KN3Ws0uIDvOmSkcR/multispectral-index-formulas
---

# Công thức đa số chỉ

Các công thức chỉ định bên dưới sử dụng phạm vi truyền tải trung bình hợp lý của bộ lọc Survey3:

<table><thead><tr><thalign="center">Bộ lọc màu Survey3</th><th width="196.199951171875"align="center">Bộ lọc tên Survey3</th><th width="159.800048828125"align="center">Phạm vi truyền tải (FWHM)</th><thalign="center">Truyền trung bình</th></tr></thead><tbody><tr><tdalign="center">Blue</td><tdalign="center">NGB - Blue</td><tdalign="center">468-483nm</td><tdalign="center">475nm</td></tr><tr><tdalign="center">Cyan</td><tdalign="center">OCN- Cyan</td><tdalign="center">476-512nm</td><tdalign="center">494nm</td></tr><tr><tdalign="center">Green</td><td căn chỉnh="center">RGN | NGB - Green</td><tdalign="center">543-558nm</td><tdalign="center">547nm</td></tr><tr><tdalign="center">Orange</td><tdalign="center">OCN - Orange</td><tdalign="center">598-640nm</td><tdalign="center">619nm</td></tr><tr><tdalign="center">Red</td><tdalign="center">RGN - Red</td><td align="center">653-668nm</td><tdalign="center">661nm</td></tr><tr><tdalign="center">RedEdge</td><tdalign="center">Re - RedEdge</td><tdalign="center">712-735nm</td><tdalign="center">724nm</td></tr><tr><td align="center">NIR1</td><tdalign="center">OCN - NIR1</td><tdalign="center">798-848nm</td><tdalign="center">823nm</td></tr><tr><tdalign="center">NIR2</td><td cơ chỉnh="center">RGN | NGB | NIR - NIR2</td><tdalign="center">835-865nm</td><tdalign="center">850nm</td></tr></tbody></table>

Khi sử dụng các công thức này, tên có thể kết thúc bằng "\_1" hoặc "\_2", tương ứng với bộ lọc NIR nào, NIR1 hoặc NIR2 đã được sử dụng.

***

## EVI - Chỉ số thực vật tăng cường

Số này ban đầu được phát triển để sử dụng với dữ liệu MODIS như một cải tiến so với NDVI bằng cách sử dụng vật liệu tín hiệu tối ưu hóa ở những khu vực có chỉ số tích phân lá cao (LAI). Nó hữu ích nhất ở các vùng LAI cao nơi NDVI có bão hòa. Nó sử dụng vùng phản xạ màu xanh lam để hiệu chỉnh đất nền tín hiệu và giảm ảnh hưởng của khí quyển, bao gồm cả khí khí xạ xạ.

$$
EVI = 2.5 *  {(NIR - Red) \over (NIR + 6 * Red - 7.5 * Blue + 1)}
$$

Giá trị EVI phải nằm trong khoảng từ 0 đến 1 đối với thực vật pixel. Các đối tượng sáng như mây và tòa nhà màu trắng, cùng với các đối tượng tối như nước, có thể dẫn đến pixel giá trị bất ngờ trong hình ảnh EVI. Trước khi tạo hình ảnh EVI, bạn nên kiểm tra các đám mây và các đặc điểm sáng khỏi hình ảnh phản chiếu và tùy ý đặt ngưỡng các pixel giá trị từ 0 đến 1.

_Tài liệu tham khảo: Huete, A., et al. "Tổng quan về hiệu suất đo xạ và sinh thái của các thực vật MODIS." Viễn thám môi trường 83 (2002):195–213._

***

## FCI1 - Chỉ số độ che phủ rừng 1

Chỉ số này phân biệt các loài rừng với các loại thảm thực vật khác bằng cách sử dụng hình ảnh phản xạ đa phổ bao gồm dải viền màu đỏ.

$$
FCI1 = Red * RedEdge
$$

Các khu vực có rừng sẽ có giá trị FCI1 thấp hơn so với độ phản chiếu của cây thấp hơn và sự hiện diện của bóng trong tán cây.

_Tài liệu tham khảo: Becker, Sarah J., Craig S.T. Daughtry và Andrew L. Russ. "Chỉ số che phủ mạnh mạnh cho hình ảnh đa quang phổ." Kỹ thuật đo ảnh và viễn thám 84.8 (2018): 505-512._

***

## FCI2 - Chỉ số độ che phủ rừng 2

Chỉ số này phân biệt các loài rừng với các loại thảm thực vật khác bằng cách sử dụng hình ảnh phản xạ đa phổ không bao gồm dải viền màu đỏ.

$$
FCI2 = Red * NIR
$$

Các khu vực rừng sẽ có giá trị FCI2 thấp hơn so với độ phản chiếu của cây thấp hơn và sự hiện diện của bóng trong tán cây.

_Tài liệu tham khảo: Becker, Sarah J., Craig S.T. Daughtry và Andrew L. Russ. "Chỉ số che phủ mạnh mạnh cho hình ảnh đa quang phổ." Kỹ thuật đo ảnh và viễn thám 84.8 (2018): 505-512._

***

## GEMI - Chỉ số giám sát môi trường toàn cầu

Đây là một số thực phẩm phi tuyến tính được sử dụng để giám sát môi trường toàn cầu từ hình ảnh bảo vệ và cố gắng điều chỉnh các ứng dụng khí quyển. Nó tương tự NDVI nhưng ít nhạy cảm hơn so với các ứng dụng khí quyển. Nó bị ảnh hưởng bởi mặt đất trống; làm như vậy sẽ không được khuyến khích sử dụng ở những khu vực có thảm thực thưa thớt hoặc dày đặc phải.

$$
GEMI = eta (1 - 0.25 * eta) - {Red - 0.125 \over 1 - Red}
$$

Ở đâu:

$$
eta = {2(NIR^{2}-Red^{2}) + 1.5 * NIR + 0.5 *  Red \over NIR + Red + 0.5}
$$

_Tài liệu tham khảo: Pinty, B., và M. Verstraete. GEMI: Chỉ số phi tuyến tính để theo dõi thảm thực vật toàn cầu từ vệ tinh. thân thực vật 101 (1992): 15-20._

***

## GARI - Green Chỉ số chống khí quyển

Chỉ số này nhạy cảm hơn nhiều nồng độ chất diệp lục và ít nhạy cảm hơn so với các loại khí cụ ứng dụng như NDVI.

$$
GARI = {NIR - [Green - \gamma(Blue - Red)] \over NIR + [Green - \gamma(Blue - Red)]   }
$$

Hằng số gamma là một hàm phụ quan trọng phụ thuộc vào điều kiện mặt trời trong khí khí quyển. ENVI use value 1,7, is value do Gitelson, Kaufman và Merzylak đề xuất (1996, trang 296).

_Tài liệu tham khảo: Gitelson, A., Y. Kaufman và M. Merzylak. "Sử dụng Kênh Green trong Viễn tưởng thảm thực vật toàn cầu từ EOS-MODIS." Viễn thám môi trường 58 (1996): 289-298._

***

##GCI - Chỉ số diệp Green

Số này chỉ được sử dụng để ước tính hàm lượng chất diệp lục trong lá của nhiều loài thực vật.

$$
GCI = {NIR \over Green} - 1
$$

Việc có NIR width và bước sóng màu xanh lá cây giúp dự kiến ​​​​tốt hơn về đồng thời chức năng phân loại chất lượng cho phép tăng độ nhạy và tỷ lệ tín hiệu nhiễu cao hơn.

_Tài liệu tham khảo: Gitelson, A., Y. Gritz và M. Merzlyak. "Mối quan hệ giữa hàm lượng chất diệp lục trong lá và mức độ phản xạ phổ phổ xạ cũng như các thuật toán chiến đấu giá chất diệp lục không bị phá hủy ở lá cây cao hơn." Tạp chí Sinh lý học Thực vật 160 (2003): 271-282._

***

## GLI - Chỉ số lá Green

Chỉ số này ban đầu được thiết kế để sử dụng với máy ảnh kỹ thuật số RGB để đo độ che phủ lúa mì, trong đó các kỹ thuật số (DN) màu đỏ, lục và lam nằm trong khoảng từ 0 đến 255.

$$
GLI = {(Green - Red) + (Green - Blue)  \over (2 * Green) + Red + Blue }
$$

Giá trị GLI nằm trong khoảng từ -1 đến +1. Giá trị âm đại diện cho đất và các đặc điểm không tồn tại sự sống, trong khi giá trị dương đại diện cho lá và thân xanh.

_Tài liệu tham khảo: Louhaichi, M., M. Borman, và D. Johnson. "Nền tảng được bố trí theo không gian và chụp ảnh trên không để ghi lại tác động của công việc thư giãn với lúa mì." Geocarto International 16, số 1 (2001): 65-70._

***

## GNDVI - Green Chỉ số thực vật đặc biệt chuẩn hóa

Chỉ số này tương tự NDVI ngoại trừ việc nó đo phổ màu xanh lá cây từ 540 đến 570 nm thay vì phổ màu đỏ. Chỉ số này nhạy cảm hơn so với NDVI.

$$
GNDVI = {(NIR - Green) \over (NIR + Green)  }
$$

_Tài liệu tham khảo: Gitelson, A., và M. Merzlyak. "Cảm biến từ xa về nồng độ chất diệp lục trong lá cây cao hơn." Những tiến bộ trong nghiên cứu không gian 22 (1998): 689-692._

***

## GOSAVI - Green Chỉ số thực vật điều chỉnh đất tối ưu

Chỉ số này ban đầu được thiết kế bằng kỹ thuật chụp ảnh màu hồng ngoại màu để mong đợi nhu cầu rồng cho ngô. Nó tương tự như OSAVI, nhưng nó thay thế dải màu xanh lá cây bằng màu đỏ.

$$
GOSAVI = {NIR - Green \over NIR + Green + 0.16)  }
$$

_Tài liệu tham khảo: Sripada, R., et al. "Xác định nhu cầu sâu cho ngô bằng cách sử dụng ảnh hồng ngoại màu trên không." Tiến sĩ luận luận, Đại học bang Bắc Carolina, 2005._

***

##GRVI - Chỉ số thực vật Green

Chỉ số này rất nhạy cảm với tốc độ quang hợp trong rừng hoang dã, vì tốc độ phản xạ màu xanh lá cây và màu đỏ đã ảnh hưởng mạnh mẽ bởi sự thay đổi sắc tố của lá.

$$
GRVI = {NIR \over Green }
$$

_Tài liệu tham khảo: Sripada, R., et al. "Chụp ảnh hồng ngoại màu trên không để xác định nhu cầu trầm đầu mùa ở ngô." Tạp chí Nông học 98 (2006): 968-977._

***

## GSAVI - Green Chỉ số thực vật điều chỉnh đất

Chỉ số này ban đầu được thiết kế bằng kỹ thuật chụp ảnh màu hồng ngoại màu để mong đợi nhu cầu rồng cho ngô. Nó tương tự như SAVI, nhưng nó thay thế dải màu xanh lá cây bằng màu đỏ.

$$
GSAVI = 1.5 * {(NIR - Green) \over (NIR + Green + 0.5)  }
$$

_Tài liệu tham khảo: Sripada, R., et al. "Xác định nhu cầu sâu cho ngô bằng cách sử dụng ảnh hồng ngoại màu trên không." Tiến sĩ luận luận, Đại học bang Bắc Carolina, 2005._

***

## LAI - Chỉ số tích lá

Số này chỉ được sử dụng để ước tính mức độ che phủ của tán lá và dự báo tốc độ tăng trưởng và năng suất cây trồng. ENVI tính toán LAI màu xanh lá cây bằng cách sử dụng công thức thực nghiệm sau đây của Boegh và cộng sự (2002):

$$
LAI = 3.618 * EVI - 0.118
$$

Trong đó EVI là:

$$
EVI = 2.5 *  {(NIR - Red) \over (NIR + 6 * Red - 7.5 * Blue + 1)}
$$

Giá trị LAI cao thường nằm trong khoảng từ 0 đến 3,5. Tuy nhiên, khi cảnh có đám mây và các đặc điểm sáng khác tạo ra các điểm ảnh hòa hòa, giá trị LAI có thể vượt quá 3,5. Lý tưởng nhất là bạn nên che đi đám mây và các đặc điểm sáng khỏi cảnh của mình trước khi tạo hình ảnh LAI.

_Tham khảo: Boegh, E., H. Soegaard, N. Broge, C. Hasager, N. Jensen, K. Schelde, và A. Thomsen. "Dữ liệu đa phổ trong không khí để định lượng chỉ số tích khí, nồng độ sâu và hiệu quả quang hợp trong nông nghiệp." Viễn thông về môi trường 81, không. 2-3 (2002): 179-193._

***

##LCI - Chỉ số diệp lục

Chỉ số này được sử dụng để ước tính hàm lượng chất diệp lục ở thực vật cấp cao, nhạy cảm với sự thay đổi mức độ phản xạ do hấp thụ chất diệp lục gây nguy hiểm.

$$
LCI = {NIR2 - RedEdge \over NIR2 + Red}
$$

_Tham khảo: Datt, B. "Cảm biến từ xa về hàm lượng nước trong đàn lá bạch." Tạp chí Sinh lý học Thực vật 154, số 1. 1 (1999): 30-36._

***

## MNLI - Chỉ số phi tuyến tính được sửa đổi

Chỉ số này là cải tiến cho Chỉ số phi tuyến tính (NLI) kết hợp với Chỉ số thực vật được điều chỉnh theo đất (SAVI) để tính đến nền đất. ENVI sử dụng độ phân giải nền hệ thống giá trị (_L_) là 0,5.

$$
MNLI = {(NIR^{2} - Red) * (1 + L) \over (NIR^{2} + Red + L)  }
$$

_Người tham khảo: Yang, Z., P. Willis và R. Mueller. "Tác động của hình ảnh AWIFS nâng cao tỷ lệ băng tần đến độ chính xác của việc cắt loại." Kỷ yếu của Hội nghị chuyên đề Viễn thám Pecora 17 (2008), Denver, CO._

***

## MSAVI2 - Chỉ số thực điều chỉnh đất biến tính 2

Đây chỉ là phiên bản đơn giản hơn của chỉ số MSAVI do Qi và cộng sự (1994) đề xuất, cải tiến dựa trên Chỉ số thảm thực vật được điều chỉnh theo đất (SAVI). Nó làm giảm tiếng ồn của đất và tăng phạm vi hoạt động của tín hiệu thực tế. MSAVI2 dựa trên phương pháp nạp không sử dụng giá trị _L_ không đổi (như với SAVI) để làm nổi bật thảm thực vật khỏe mạnh.

$$
MSAVI2 = {2 * NIR + 1 - \sqrt{(2 * NIR + 1)^{2} - 8(NIR - Red)} \over 2}
$$

_Tài liệu tham khảo: Qi, J., A. Chehbouni, A. Huete, Y. Kerr và S. Sorooshian. "Chỉ thảm thực vật được điều chỉnh trên đất đã được chỉnh sửa." Viễn thám môi trường 48 (1994): 119-126._

***

## NDRE-Sự khác biệt được chuẩn hóa RedEdge

Chỉ số này tương tự NDVI nhưng so sánh độ tương phản giữa NIR với RedEdge thay vì Red, thường phát hiện căng thẳng thực tế sớm hơn.

$$
NDRE = {NIR - RedEdge \over NIR + RedEdge  }
$$

***

## NDVI - Chỉ số thực vật đặc biệt chuẩn hóa

Chỉ số này là thước đo thảm thực vật xanh, khỏe mạnh. Sự kết hợp giữa các công thức khác nhau được chuẩn hóa và sử dụng các vùng hấp thụ và phản xạ cao nhất của chất diệp lục làm cho nó bền bỉ trong nhiều điều kiện. Tuy nhiên, nó có thể gây bão hòa trong điều kiện thảm thực vật dày đặc khi LAI trở nên cao.

$$
NDVI = {NIR - Red \over NIR + Red  }
$$

Giá trị của dao động số này từ -1 đến 1. Phạm vi phổ biến của thảm thực vật xanh là 0,2 đến 0,8.

_Tài liệu tham khảo: Rouse, J., R. Haas, J. Schell và D. Deering. Giám sát hệ thống thực tế ở vùng đồng bằng ERTS. Hội nghị chuyên đề ERTS lần thứ ba, NASA (1973): 309-317._

***

##NLI - Chỉ số phi tuyến tính

Chỉ số này giả định rằng mối quan hệ giữa nhiều vật chất duy nhất và số lượng sinh vật lý giải bề mặt là phi tuyến tính. Nó tuyến tính hóa các mối quan hệ với các tham số bề mặt có xu hướng phi tuyến tính.

$$
NLI = {NIR^{2} - Red \over NIR^{2} + Red  }
$$

_Tài liệu tham khảo: Goel, N., và W. Qin. "Ảnh hưởng của kiến ​​trúc đến mối quan hệ giữa các thực vật khác nhau và LAI và Fpar: Mô phỏng máy tính." Đánh giá viễn thám 10 (1994): 309-347._

***

## OSAVI - Chỉ số thực vật điều chỉnh đất tối ưu

Chỉ số này dựa trên chỉ số thảm thực vật được điều chỉnh theo đất (SAVI). Nó sử dụng tiêu chuẩn giá trị 0,16 cho độ phân giải nền điều chỉnh hệ thống. Rondeaux (1996) đã xác định rằng giá trị này mang lại sự biến đổi đất lớn hơn SAVI đối với mức độ che phủ thực tế, đồng thời có thể tăng cường độ nhạy cảm đối với mức độ che phủ thực tế lớn hơn 50%. Chỉ số này được sử dụng tốt nhất ở những khu vực có thảm thực vật tương đối thưa thớt, nơi có thể nhìn thấy đất qua hoa cây.

$$
OSAVI = {(NIR - Red) \over (NIR + Red + 0.16)  }
$$

_Tài liệu tham khảo: Rondeaux, G., M. Steven và F. Baret. "Ưu tiên hóa các điều chỉnh thực vật chỉ số." Viễn thám môi trường 55 (1996): 95-107._

***

##RDVI - Chỉ số thực vật khác biệt chuẩn hóa

Chỉ số này sử dụng sự khác biệt giữa bước sóng hồng ngoại và bước sóng đỏ, cùng với NDVI, để làm nổi thảm thực vật khỏe mạnh. Nó không nhạy cảm với hoạt động của hình học xem đất và mặt trời.

$$
RDVI = {(NIR- Red) \over \sqrt{(NIR + Red)}  }
$$

_Tài liệu tham khảo: Roujean, J., và F. Breon. "Ước tính cải cách hành chính được hấp thụ bởi thảm thực vật từ phép đo phản xạ hai chiều." Viễn thám môi trường 51 (1995): 375-384._

***

## SAVI - Chỉ số thực vật điều chỉnh đất

Chỉ số này tương tự như NDVI, nhưng không ngăn chặn tác động của các pixel đất. Nó sử dụng hệ thống điều chỉnh nền số, _L_, là một hàm của thực tế mật khẩu và thường yêu cầu kiến ​​thức trước về thực phẩm số lượng. Huete (1988) đề xuất mức độ ưu tiên giá trị _L_=0,5 để tính toán những biến đổi nền đất bậc nhất. Chỉ số này được sử dụng tốt nhất ở những khu vực có thảm thực vật tương đối thưa thớt, nơi có thể nhìn thấy đất qua hoa cây.

$$
SAVI = {1.5 * (NIR- Red) \over (NIR + Red + 0.5)  }
$$

_Tham khảo: Huete, A. "Chỉ số thực thích nghi với đất (SAVI)." Viễn thám môi trường 25 (1988): 295-309._

***

## TDVI - Chỉ số thực vật thay đổi đặc biệt

Chỉ số này rất hữu ích cho việc theo dõi mức độ che phủ của thảm thực vật trong môi trường đô thị. Nó không bão hòa như NDVI và SAVI.

$$
TDVI = 1.5 * {(NIR- Red) \over \sqrt{NIR^{2} + Red + 0.5}  }
$$

_Tài liệu tham khảo: Bannari, A., H. Asalhi và P. Teillet. "Chỉ số thực vật khác biệt được chuyển đổi (TDVI) để lập bản đồ lớp phủ thực vật" Trọng Kỷ yếu của Hội nghị đề xuất chuyên đề về khoa học địa chất và Viễn thám, IGARSS '02, IEEE International, Tập 5 (2002)._

***

## VARI - Chỉ số chịu khí quyển có thể nhìn thấy

Chỉ số này dựa trên ARVI và được sử dụng để ước tính tỷ lệ thực tế trong một cảnh có độ nhạy đối với các hiệu ứng khí quyển.

$$
VARI = {Green - Red \over Green + Red - Blue  }
$$

_Tài liệu tham khảo: Gitelson, A., et al. " Thực vật và các đường đất trong không gian quang phổ nhìn thấy được: Khái niệm và kỹ thuật ước tính từ xa tỷ lệ thực vật. Tạp chí quốc tế về Viễn tưởng 23 (2002): 2537−2562._

***

## WDRVI - Chỉ số thực phạm vi động rộng

Chỉ số này tương tự như NDVI, nhưng nó sử dụng hệ số quan trọng (_a_) để giảm độ chênh lệch giữa xu hướng của tín hiệu hồng ngoại và tín hiệu đỏ cho NDVI. WDRVI hiệu quả đặc biệt ở những cảnh có vật thực mật khẩu từ trung bình đến cao khi NDVI vượt quá 0,6. NDVI có xu hướng chữ lại khi tỷ lệ thực vật và chỉ số tích phân lá (LAI) tăng lên, trong khi WDRVI nhạy cảm hơn phạm vi phân thực vật chất hơn và với những thay đổi trong LAI.

$$
WDRVI = {(\alpha * NIR- Red) \over (\alpha * NIR + Red)}
$$

Hệ số quan trọng (_a_) có thể dao động từ 0,1 đến 0,2. Giá trị 0,2 được Henebry, Viña và Gitelson (2004) khuyến nghị.

_Tài liệu tham khảo_

_Gitelson, A. "Chỉ số thực vật phạm vi động để định lượng từ xa các đặc tính sinh lý của thực vật." Tạp chí Sinh lý học Thực vật 161, số 2 (2004): 165-173._

_Henebry, G., A. Viña và A. Gitelson. "Chỉ số thực phẩm phạm vi rộng rãi và tiện ích của nó để phân tích khoảng cách." Bản tin phân tích khoảng trống số 12: 50-56._