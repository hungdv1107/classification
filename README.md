# Phân loại văn bản tiếng Việt

![](https://img.shields.io/badge/made%20with-%E2%9D%A4-red.svg)
![](https://img.shields.io/badge/opensource-vietnamese-blue.svg)
![](https://img.shields.io/badge/build-passing-green.svg)

Dự án nghiên cứu về bài toán *phân loại văn bản tiếng Việt*, được phát triển bởi nhóm nghiên cứu xử lý ngôn ngữ tự nhiên tiếng Việt - [underthesea](https://github.com/undertheseanlp). Chứa mã nguồn các thử nghiệm cho việc xử lý dữ liệu, huấn luyện và đánh giá mô hình, cũng như cho phép dễ dàng tùy chỉnh mô hình đối với những tập dữ liệu mới.

## Kết quả thử nghiệm 

Xem thêm về [*mô tả dữ liệu*](https://github.com/JackNhat/classification/blob/master/data_format.md)

Kết quả các thử nghiệm kết hợp mô hình SVM và các đặc trưng Tfidf, BoW

| Mô hình                                         | F1 %     |
|-------------------------------------------------|----------|
| TfidfVectorizer(ngram_range=(1, 2), max_df=0.5) | **92.8** |
| CountVectorizer(ngram_range=(1, 3), max_df=0.7) | 89.3     |
| TfidfVectorizer(max_df=0.8)                     | 89.0     |
| CountVectorizer(ngram_range=(1, 3)              | 88.9     |
| TfidfVectorizer(ngram_range=(1, 3))             | 86.8     |
| CountVectorizer(max_df=0.7)                     | 85.5     |

## Thiết lập môi trường

**Yêu cầu hệ thống** 

* `Hệ điều hành: Linux (Ubuntu, CentOS), Mac`
* `Python 3.6+`
* `conda 4+`

**Cài đặt**

```
# Tải project bằng cách sử dụng lệnh `git clone`
$ git clone https://github.com/undertheseanlp/classification.git

# Tạo môi trường mới và cài đặt các gói liên quan
$ cd classification
$ conda create -n classification python=3.6
$ pip install -r requirements.txt
```

## Hướng dẫn sử dụng

Trước khi chạy các thử nghiệm, hãy chắc chắn bạn đã activate môi trường `classification`, mọi câu lệnh đều được chạy trong thư mục gốc của dự án.

```
$ cd classification
$ source activate classification
```
**Sử dụng mô hình có sẵn**

Dự đoán nhãn của một câu:

```
$ python classification.py "Trong phần lớn thời gian sáng nay, thầy trò HLV Park Hang Seo có cuộc họp nội bộ tại khách sạn. Tại cuộc họp này, nội dung quan trọng nhất chính là xem băng ghi hình một số trận đấu gần nhất của đội chủ nhà, giúp đội tuyển Việt Nam đưa ra những phương án đối phó trong trận ra quân tại AFF Cup 2018 sắp tới."
The thao
```

Dự đoán nhãn từ nội dung trong file, sử dụng tùy chọn `--fin`

```
$ python classification.py \
    --fin tmp/input.txt
Kinh doanh
```

### Huấn luyện mô hình

**Tiền xử lý dữ liệu**

Lấy về bộ dữ liệu VNTC
```
$ sh util/get_vntc.sh
```

Tập dữ liệu được quy chuẩn từng nhãn theo từng danh mục chứa các file văn bản như tập dữ liệu trong thư mục data/raw


Chuyển đổi tập dữ liệu thành file excel

```
$ python util/preprocess_vntc.py
```

**So sánh các thử nghiệm**

Các thử nghiệm kết hợp LinearSVC và CountVectorizer 

```
$  python optimize_hyperparameters.py --mode optimize 
            --train data/corpus/train.xlsx 
            --test data/corpus/test.xlsx 
            --trans tfidf 
```

```
$ python optimize_hyperparameters.py --mode optimize 
            --train data/corpus/train.xlsx
            --test data/corpus/test.xlsx 
            --trans count 

```

**Huấn luyện và lưu mô hình**

```
$ python train.py --mode train-test 
            --train data/corpus/train.xlsx 
            --test data/corpus/test.xlsx 
            --s models
```

### Sử dụng mô hình đã huấn luyện


Dự đoán nhãn của một câu:

```
$ python classification.py "Trong suốt kỳ chuyển nhượng mùa hè qua, tiền vệ Eden Hazard của Chelsea đã luôn được Real Madrid nhắm đến để thay thế Cristiano Ronaldo nhưng bất thành. Mới đây, Hazard đã cho biết anh đang chờ đợi thêm những tín hiệu chiêu mộ từ Real Madrid trước khi đưa ra quyết định về tương lai của mình ở Chelsea."
Bong da
```

Dự đoán nhãn từ nội dung trong file, sử dụng tùy chọn `--fin`

```
$ python classification.py \
    --fin tmp/input.txt
Giao duc
```

## Trích dẫn

Nếu bạn sử dụng mã nguồn này, xin hãy trích dẫn thông tin sau 

```
@online{undertheseanlp/classification,
author ={Vu Anh, Pham Hong Quang},
year = {2019},
title ={Phân loại văn bản tiếng Việt},
url ={https://github.com/undertheseanlp/classification}
}
```

## Lời cảm ơn

Xin chân thành cảm ơn các nhóm phát triển sklearn, fasttext đã tạo ra những công cụ hữu ích để nhóm sử dụng trong các thử nghiệm của mình.

## Bản quyền

Mã nguồn của dự án được phân phối theo giấy phép [GPL-3.0](LICENSE.txt).

Dự án sử dụng tập dữ liệu **[VNTC](https://github.com/duyvuleo/VNTC)** trong các thử nghiệm. Xin vui lòng kiểm tra lại thông tin trên website hoặc báo cáo khoa học tương ứng để biết thông tin về bản quyền và trích dẫn khi sử dụng tập dữ liệu này.
