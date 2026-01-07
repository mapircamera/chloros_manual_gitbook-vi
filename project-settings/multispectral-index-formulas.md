---
mô tả: Trang này liệt kê một số chỉ số đa phổ mà Chloros sử dụng
liên kết meta:
  alternates:
    - >-
      https://app.gitbook.com/s/o044KN3Ws0uIDvOmSkcR/multispectral-index-formulas
---

# Công thức chỉ số đa phổ

Các công thức chỉ số bên dưới sử dụng kết hợp phạm vi truyền trung bình của bộ lọc Survey3:

<table><thead><tr><th align="center">Survey3 Filter Color</th><th width="196.199951171875" align="center">Survey3 Filter Name</th><th width="159.800048828125" align="center">Transmission Range (FWHM)</th><th align="center">Average Transmission</th></tr></thead><tbody><tr><td align="center">Blue</td><td align="center">NGB - Blue</td><td align="center">468-483nm</td><td align="center">475nm</td></tr><tr><td align="center">Cyan</td><td align="center">OCN- Cyan</td><td align="center">476-512nm</td><td align="center">494nm</td></tr><tr><td align="center">Green</td><td align="center">RGN | NGB - Green</td><td align="center">543-558nm</td><td align="center">547nm</td></tr><tr><td align="center">Orange</td><td align="center">OCN - Orange</td><td align="center">598-640nm</td><td align="center">619nm</td></tr><tr><td align="center">Red</td><td align="center">RGN - Red</td><td align="center">653-668nm</td><td align="center">661nm</td></tr><tr><td align="center">RedEdge</td><td align="center">Re - RedEdge</td><td align="center">712-735nm</td><td align="center">724nm</td></tr><tr><td align="center">NIR1</td><td align="center">OCN - NIR1</td><td align="center">798-848nm</td><td align="center">823nm</td></tr><tr><td align="center">NIR2</td><td align="center">RGN | NGB | NIR - NIR2</td><td align="center">835-865nm</td><td align="center">850nm</td></tr></tbody></table>

Khi sử dụng các công thức này, tên có thể kết thúc bằng "\_1" hoặc "\_2", tương ứng với bộ lọc NIR nào, NIR1 hoặc NIR2 đã được sử dụng.

***

## EVI - Chỉ số thực vật tăng cường

Chỉ số này ban đầu được phát triển để sử dụng với dữ liệu MODIS như một sự cải tiến so với NDVI bằng cách tối ưu hóa tín hiệu thực vật ở những khu vực có chỉ số diện tích lá cao (LAI). Nó hữu ích nhất ở các vùng LAI cao, nơi NDVI có thể bão hòa. Nó sử dụng vùng phản xạ màu xanh lam để hiệu chỉnh các tín hiệu nền đất và giảm ảnh hưởng của khí quyển, bao gồm cả sự tán xạ sol khí.

$$
EVI = 2.5 *  {(NIR - Red) \over (NIR + 6 * Red - 7.5 * Blue + 1)}
$$

Giá trị EVI phải nằm trong khoảng từ 0 đến 1 đối với pixel thực vật. Các đối tượng sáng như mây và tòa nhà màu trắng, cùng với các đối tượng tối như nước, có thể dẫn đến giá trị pixel bất thường trong hình ảnh EVI. Trước khi tạo hình ảnh EVI, bạn nên che các đám mây và các đặc điểm sáng khỏi hình ảnh phản chiếu và tùy ý đặt ngưỡng các giá trị pixel từ 0 đến 1.

_Tài liệu tham khảo: Huete, A., et al. "Tổng quan về hiệu suất đo phóng xạ và sinh lý của các chỉ số thực vật MODIS." Viễn thám môi trường 83 (2002):195–213._

***

## FCI1 - Chỉ số độ che phủ rừng 1

Chỉ số này phân biệt các tán rừng với các loại thảm thực vật khác bằng cách sử dụng hình ảnh phản xạ đa phổ bao gồm dải viền màu đỏ.

$$
FCI1 = Red * RedEdge
$$

Các khu vực có rừng sẽ có giá trị FCI1 thấp hơn do độ phản chiếu của cây thấp hơn và sự hiện diện của bóng trong tán cây.

_Tài liệu tham khảo: Becker, Sarah J., Craig S.T. Daughtry và Andrew L. Russ. "Chỉ số che phủ rừng mạnh mẽ cho hình ảnh đa quang phổ." Kỹ thuật đo ảnh & Viễn thám 84.8 (2018): 505-512._

***

## FCI2 - Chỉ số độ che phủ rừng 2

Chỉ số này phân biệt các tán rừng với các loại thảm thực vật khác bằng cách sử dụng hình ảnh phản xạ đa phổ không bao gồm dải viền màu đỏ.

$$
FCI2 = Red * NIR
$$

Các khu vực có rừng sẽ có giá trị FCI2 thấp hơn do độ phản xạ của cây thấp hơn và sự hiện diện của bóng trong tán cây.

_Tài liệu tham khảo: Becker, Sarah J., Craig S.T. Daughtry và Andrew L. Russ. "Chỉ số che phủ rừng mạnh mẽ cho hình ảnh đa quang phổ." Kỹ thuật đo ảnh & Viễn thám 84.8 (2018): 505-512._

***

## GEMI - Chỉ số giám sát môi trường toàn cầu

Chỉ số thực vật phi tuyến tính này được sử dụng để giám sát môi trường toàn cầu từ hình ảnh vệ tinh và cố gắng điều chỉnh các hiệu ứng khí quyển. Nó tương tự như NDVI nhưng ít nhạy cảm hơn với các hiệu ứng khí quyển. Nó bị ảnh hưởng bởi đất trống; do đó, nó không được khuyến khích sử dụng ở những khu vực có thảm thực vật thưa thớt hoặc dày đặc vừa phải.

$$
GEMI = eta (1 - 0.25 * eta) - {Red - 0.125 \over 1 - Red}
$$

Ở đâu:

$$
eta = {2(NIR^{2}-Red^{2}) + 1.5 * NIR + 0.5 *  Red \over NIR + Red + 0.5}
$$

_Tài liệu tham khảo: Pinty, B., và M. Verstraete. GEMI: Chỉ số phi tuyến tính để theo dõi thảm thực vật toàn cầu từ vệ tinh. Thảm thực vật 101 (1992): 15-20._

***##GARI - Chỉ số chống chịu khí quyển xanh

Chỉ số này nhạy hơn với phạm vi nồng độ diệp lục rộng và ít nhạy cảm hơn với các tác động của khí quyển so với NDVI.

$$
GARI = {NIR - [Green - \gamma(Blue - Red)] \over NIR + [Green - \gamma(Blue - Red)]   }
$$

Hằng số gamma là một hàm trọng số phụ thuộc vào điều kiện sol khí trong khí quyển. ENVI sử dụng giá trị 1,7, là giá trị được đề xuất bởi Gitelson, Kaufman và Merzylak (1996, trang 296).

_Tài liệu tham khảo: Gitelson, A., Y. Kaufman và M. Merzylak. "Sử dụng Kênh Xanh trong Viễn thám thảm thực vật Toàn cầu từ EOS-MODIS." Viễn thám môi trường 58 (1996): 289-298._***

##GCI - Chỉ số diệp lục xanh

Chỉ số này được sử dụng để ước tính hàm lượng chất diệp lục trong lá của nhiều loài thực vật.

$$
GCI = {NIR \over Green} - 1
$$

Việc có bước sóng NIR rộng và màu xanh lá cây giúp dự đoán tốt hơn về hàm lượng chất diệp lục đồng thời cho phép độ nhạy cao hơn và tỷ lệ tín hiệu trên nhiễu cao hơn.

_Tài liệu tham khảo: Gitelson, A., Y. Gritz và M. Merzlyak. "Mối quan hệ giữa hàm lượng chất diệp lục trong lá và độ phản xạ quang phổ cũng như các thuật toán đánh giá chất diệp lục không phá hủy ở lá cây cao hơn." Tạp chí Sinh lý học Thực vật 160 (2003): 271-282._***

##GLI - Chỉ Số Lá Xanh

Chỉ số này ban đầu được thiết kế để sử dụng với máy ảnh RGB kỹ thuật số để đo độ che phủ lúa mì, trong đó các số kỹ thuật số (DN) màu đỏ, xanh lá cây và xanh lam nằm trong khoảng từ 0 đến 255.

$$
GLI = {(Green - Red) + (Green - Blue)  \over (2 * Green) + Red + Blue }
$$

Giá trị GLI nằm trong khoảng từ -1 đến +1. Giá trị âm đại diện cho đất và các đặc điểm không có sự sống, trong khi giá trị dương đại diện cho lá và thân xanh.

_Tài liệu tham khảo: Louhaichi, M., M. Borman, và D. Johnson. "Nền tảng được bố trí theo không gian và chụp ảnh trên không để ghi lại tác động của việc chăn thả đối với lúa mì." Geocarto International 16, số 1 (2001): 65-70._

***## GNDVI - Chỉ số thực vật khác biệt chuẩn hóa xanh

Chỉ số này tương tự như NDVI ngoại trừ việc nó đo phổ màu xanh lá cây từ 540 đến 570 nm thay vì phổ màu đỏ. Chỉ số này nhạy cảm hơn với nồng độ diệp lục so với NDVI.

$$
GNDVI = {(NIR - Green) \over (NIR + Green)  }
$$

_Tài liệu tham khảo: Gitelson, A., và M. Merzlyak. "Cảm biến từ xa về nồng độ chất diệp lục trong lá cây cao hơn." Những tiến bộ trong nghiên cứu không gian 22 (1998): 689-692._***

## GOSAVI - Chỉ số thực vật điều chỉnh đất được tối ưu hóa xanh

Chỉ số này ban đầu được thiết kế bằng kỹ thuật chụp ảnh hồng ngoại màu để dự đoán nhu cầu nitơ cho ngô. Nó tương tự như OSAVI, nhưng nó thay thế dải màu xanh lá cây bằng màu đỏ.

$$
GOSAVI = {NIR - Green \over NIR + Green + 0.16)  }
$$

_Tài liệu tham khảo: Sripada, R., et al. "Xác định nhu cầu nitơ trong mùa đối với ngô bằng cách sử dụng ảnh hồng ngoại màu trên không." Tiến sĩ luận án, Đại học bang Bắc Carolina, 2005._***## GRVI - Chỉ số thực vật tỷ lệ xanh

Chỉ số này rất nhạy cảm với tốc độ quang hợp trong tán rừng, vì độ phản xạ màu xanh lá cây và màu đỏ bị ảnh hưởng mạnh mẽ bởi sự thay đổi sắc tố của lá.

$$
GRVI = {NIR \over Green }
$$

_Tài liệu tham khảo: Sripada, R., et al. "Chụp ảnh hồng ngoại màu trên không để xác định nhu cầu nitơ đầu mùa ở ngô." Tạp chí Nông học 98 (2006): 968-977._***

## GSAVI - Chỉ số thực vật điều chỉnh đất xanh

Chỉ số này ban đầu được thiết kế bằng kỹ thuật chụp ảnh hồng ngoại màu để dự đoán nhu cầu nitơ cho ngô. Nó tương tự như SAVI, nhưng nó thay thế dải màu xanh lá cây bằng màu đỏ.

$$
GSAVI = 1.5 * {(NIR - Green) \over (NIR + Green + 0.5)  }
$$

_Tài liệu tham khảo: Sripada, R., et al. "Xác định nhu cầu nitơ trong mùa đối với ngô bằng cách sử dụng ảnh hồng ngoại màu trên không." Tiến sĩ luận án, Đại học bang Bắc Carolina, 2005._

***

## LAI - Chỉ số diện tích lá

Chỉ số này được sử dụng để ước tính độ che phủ của tán lá và dự báo tốc độ tăng trưởng và năng suất cây trồng. ENVI tính toán LAI xanh bằng công thức thực nghiệm sau đây của Boegh và cộng sự (2002):

$$
LAI = 3.618 * EVI - 0.118
$$

EVI ở đâu:

$$
EVI = 2.5 *  {(NIR - Red) \over (NIR + 6 * Red - 7.5 * Blue + 1)}
$$

Giá trị LAI cao thường nằm trong khoảng từ 0 đến 3,5. Tuy nhiên, khi cảnh có mây và các đặc điểm sáng khác tạo ra điểm ảnh bão hòa, giá trị LAI có thể vượt quá 3,5. Lý tưởng nhất là bạn nên che đi các đám mây và các đặc điểm sáng khỏi cảnh của mình trước khi tạo hình ảnh LAI.

_Tham khảo: Boegh, E., H. Soegaard, N. Broge, C. Hasager, N. Jensen, K. Schelde, và A. Thomsen. "Dữ liệu đa phổ trong không khí để định lượng chỉ số diện tích lá, nồng độ nitơ và hiệu quả quang hợp trong nông nghiệp." Viễn thám về môi trường 81, không. 2-3 (2002): 179-193._

***## LCI - Chỉ số diệp lục lá

Chỉ số này được sử dụng để ước tính hàm lượng chất diệp lục ở thực vật bậc cao, nhạy cảm với sự thay đổi độ phản xạ do sự hấp thụ chất diệp lục gây ra.

$$
LCI = {NIR2 - RedEdge \over NIR2 + Red}
$$

_Tham khảo: Datt, B. "Cảm biến từ xa về hàm lượng nước trong lá bạch đàn." Tạp chí Sinh lý học Thực vật 154, số 1. 1 (1999): 30-36._***

## MNLI - Chỉ số phi tuyến tính được sửa đổi

Chỉ số này là sự cải tiến cho Chỉ số Phi tuyến tính (NLI) kết hợp với Chỉ số thảm thực vật được điều chỉnh theo đất (SAVI) để tính đến nền đất. ENVI sử dụng giá trị hệ số điều chỉnh nền tán (_L_) là 0,5.

$$
MNLI = {(NIR^{2} - Red) * (1 + L) \over (NIR^{2} + Red + L)  }
$$

_Người tham khảo: Yang, Z., P. Willis và R. Mueller. "Tác động của hình ảnh AWIFS nâng cao tỷ lệ băng tần đến độ chính xác của phân loại cắt." Kỷ yếu của Hội nghị chuyên đề viễn thám Pecora 17 (2008), Denver, CO._

***

## MSAVI2 - Chỉ số thực vật điều chỉnh đất biến tính 2

Chỉ số này là phiên bản đơn giản hơn của chỉ số MSAVI do Qi và cộng sự (1994) đề xuất, nhằm cải thiện Chỉ số thảm thực vật được điều chỉnh theo đất (SAVI). Nó làm giảm tiếng ồn của đất và tăng phạm vi động của tín hiệu thực vật. MSAVI2 dựa trên phương pháp quy nạp không sử dụng giá trị _L_ không đổi (như với SAVI) để làm nổi bật thảm thực vật khỏe mạnh.

$$
MSAVI2 = {2 * NIR + 1 - \sqrt{(2 * NIR + 1)^{2} - 8(NIR - Red)} \over 2}
$$

_Tài liệu tham khảo: Qi, J., A. Chehbouni, A. Huete, Y. Kerr và S. Sorooshian. "Chỉ số thảm thực vật được điều chỉnh trên đất đã được sửa đổi." Viễn thám môi trường 48 (1994): 119-126._

***## NDRE- Chuẩn hóa sự khác biệt RedEdge

Chỉ số này tương tự NDVI nhưng so sánh độ tương phản giữa NIR với RedEdge thay vì Red, thường phát hiện căng thẳng thực vật sớm hơn.

$$
NDRE = {NIR - RedEdge \over NIR + RedEdge  }
$$***

## NDVI - Chỉ số thực vật khác biệt chuẩn hóa

Chỉ số này là thước đo thảm thực vật xanh, khỏe mạnh. Sự kết hợp giữa công thức khác biệt được chuẩn hóa và việc sử dụng các vùng hấp thụ và phản xạ cao nhất của chất diệp lục làm cho nó bền bỉ trong nhiều điều kiện. Tuy nhiên, nó có thể bão hòa trong điều kiện thảm thực vật dày đặc khi LAI ở mức cao.

$$
NDVI = {NIR - Red \over NIR + Red  }
$$

Giá trị của chỉ số này dao động từ -1 đến 1. Phạm vi phổ biến của thảm thực vật xanh là 0,2 đến 0,8.

_Tài liệu tham khảo: Rouse, J., R. Haas, J. Schell và D. Deering. Giám sát hệ thống thực vật ở vùng đồng bằng lớn bằng ERTS. Hội nghị chuyên đề ERTS lần thứ ba, NASA (1973): 309-317._***## NLI - Chỉ số phi tuyến tính

Chỉ số này giả định rằng mối quan hệ giữa nhiều chỉ số thực vật và các thông số sinh lý bề mặt là phi tuyến tính. Nó tuyến tính hóa các mối quan hệ với các tham số bề mặt có xu hướng phi tuyến tính.

$$
NLI = {NIR^{2} - Red \over NIR^{2} + Red  }
$$

_Tài liệu tham khảo: Goel, N., và W. Qin. "Ảnh hưởng của kiến ​​trúc tán cây đến mối quan hệ giữa các chỉ số thực vật khác nhau với LAI và Fpar: Mô phỏng máy tính." Đánh giá viễn thám 10 (1994): 309-347._***

## OSAVI - Chỉ số thực vật điều chỉnh đất tối ưu

Chỉ số này dựa trên Chỉ số thảm thực vật được điều chỉnh theo đất (SAVI). Nó sử dụng giá trị tiêu chuẩn 0,16 cho hệ số điều chỉnh nền tán. Rondeaux (1996) đã xác định rằng giá trị này mang lại sự biến đổi đất lớn hơn SAVI đối với độ che phủ thực vật thấp, đồng thời thể hiện độ nhạy tăng lên đối với độ che phủ thực vật lớn hơn 50%. Chỉ số này được sử dụng tốt nhất ở những khu vực có thảm thực vật tương đối thưa thớt, nơi có thể nhìn thấy đất qua tán cây.

$$
OSAVI = {(NIR - Red) \over (NIR + Red + 0.16)  }
$$

_Tài liệu tham khảo: Rondeaux, G., M. Steven và F. Baret. "Tối ưu hóa các chỉ số thực vật điều chỉnh đất." Viễn thám môi trường 55 (1996): 95-107._***## RDVI - Chỉ số thực vật khác biệt tái chuẩn hóa

Chỉ số này sử dụng sự khác biệt giữa bước sóng cận hồng ngoại và bước sóng đỏ, cùng với NDVI, để làm nổi bật thảm thực vật khỏe mạnh. Nó không nhạy cảm với tác động của hình học xem đất và mặt trời.

$$
RDVI = {(NIR- Red) \over \sqrt{(NIR + Red)}  }
$$

_Tài liệu tham khảo: Roujean, J., và F. Breon. "Ước tính cải cách hành chính được hấp thụ bởi thảm thực vật từ các phép đo phản xạ hai chiều." Viễn thám môi trường 51 (1995): 375-384._***

##SAVI - Chỉ số thực vật điều chỉnh đất

Chỉ số này tương tự như NDVI nhưng nó ngăn chặn ảnh hưởng của các điểm ảnh đất. Nó sử dụng hệ số điều chỉnh nền tán, _L_, là một hàm của mật độ thực vật và thường đòi hỏi kiến ​​thức trước về số lượng thực vật. Huete (1988) đề xuất giá trị tối ưu _L_=0,5 để giải thích cho những biến đổi nền đất bậc nhất. Chỉ số này được sử dụng tốt nhất ở những khu vực có thảm thực vật tương đối thưa thớt, nơi có thể nhìn thấy đất qua tán cây.

$$
SAVI = {1.5 * (NIR- Red) \over (NIR + Red + 0.5)  }
$$

_Tham khảo: Huete, A. "Chỉ số thực vật điều chỉnh đất (SAVI)." Viễn thám môi trường 25 (1988): 295-309._

***

## TDVI - Chỉ số thực vật biến đổi khác biệt

Chỉ số này rất hữu ích cho việc theo dõi độ che phủ của thảm thực vật trong môi trường đô thị. Nó không bão hòa như NDVI và SAVI.

$$
TDVI = 1.5 * {(NIR- Red) \over \sqrt{NIR^{2} + Red + 0.5}  }
$$

_Tài liệu tham khảo: Bannari, A., H. Asalhi và P. Teillet. "Chỉ số thực vật khác biệt được chuyển đổi (TDVI) để lập bản đồ lớp phủ thực vật" Trong Kỷ yếu của Hội nghị chuyên đề về khoa học địa chất và viễn thám, IGARSS '02, IEEE International, Tập 5 (2002)._

***## VARI - Chỉ số chống chịu khí quyển có thể nhìn thấy

Chỉ số này dựa trên ARVI và được sử dụng để ước tính tỷ lệ thực vật trong một cảnh có độ nhạy thấp với các hiệu ứng khí quyển.

$$
VARI = {Green - Red \over Green + Red - Blue  }
$$

_Tài liệu tham khảo: Gitelson, A., et al. "Thực vật và các đường đất trong không gian quang phổ nhìn thấy được: Khái niệm và kỹ thuật ước tính từ xa tỷ lệ thực vật. Tạp chí quốc tế về viễn thám 23 (2002): 2537−2562._***

## WDRVI - Chỉ số thực vật dải động rộng

Chỉ số này tương tự như NDVI, nhưng nó sử dụng hệ số trọng số (_a_) để giảm sự chênh lệch giữa sự đóng góp của tín hiệu cận hồng ngoại và tín hiệu đỏ cho NDVI. WDRVI đặc biệt hiệu quả ở những cảnh có mật độ thực vật từ trung bình đến cao khi NDVI vượt quá 0,6. NDVI có xu hướng chững lại khi tỷ lệ thực vật và chỉ số diện tích lá (LAI) tăng lên, trong khi WDRVI nhạy cảm hơn với phạm vi rộng hơn của các phân số thực vật và những thay đổi của LAI.

$$
WDRVI = {(\alpha * NIR- Red) \over (\alpha * NIR + Red)}
$$

Hệ số trọng số (_a_) có thể dao động từ 0,1 đến 0,2. Giá trị 0,2 được Henebry, Viña và Gitelson (2004) khuyến nghị.

_Tài liệu tham khảo_

_Gitelson, A. "Chỉ số thực vật phạm vi động rộng để định lượng từ xa các đặc tính sinh lý của thực vật." Tạp chí Sinh lý học Thực vật 161, số 2 (2004): 165-173._

_Henebry, G., A. Viña và A. Gitelson. "Chỉ số thực vật phạm vi động rộng và tiện ích tiềm năng của nó để phân tích khoảng cách." Bản tin phân tích khoảng trống số 12: 50-56._
