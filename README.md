# StockRank

|              작동 화면                 |
|:------------------------------------:|
| ![](https://i.imgur.com/35IpSnh.gif) |


## 🍎 콜렉션 뷰를 생성할 때 3가지가 필요하다.

- Data → 어떤 데이터를 사용 할거야?
- Presentation → 셀을 어떻게 표현 할거야?
- Layout → 셀을 어떻게 배치할꺼야?

## 🍎 콜렉션 뷰에서 dataSource와 delegate의 역할

```swift
class StockRankViewController: UIViewController {
    @IBOutlet weak var collectionView: UICollectionView!

    override func viewDidLoad() {
        super.viewDidLoad()
        collectionView.dataSource = self
        collectionView.delegate = self
    }
}
```

- collectionView.dataSource = self → StockRankViewController가 collectionView가 사용할 data에 대해서 알려줄것이다. (Data, Presentation)
- collectionView.delegate = self → StockRankViewController가 collectionView가 어떻게 배치할건지 알려줄것이다. (Layout)

## 🍎 UICollectionViewDataSource 프로토콜 채택 후 준수하기

```swift
// dataSouce 프로토콜을 준수하려면 두가지 메서드를 구현해야한다.
extension StockRankViewController: UICollectionViewDataSource {
    
    // Data -> 셀에 들어가는 데이터를 구성
    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        guard let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "StockRankCollectionViewCell", for: indexPath) as? StockRankCollectionViewCell else {
            return UICollectionViewCell()
        }
        
        let stock = stockList[indexPath.item] // indexPath.row는 tableView에서 쓰이고, indexPath.item은 collectionView에서 쓰인다.
        cell.configure(stock)
        return cell
    }
    // Presentation -> 총 몇개의 셀이 보여지는지 구성
    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        return StockModel.list.count
    }
}
```

## 🍎 UICollectionViewDelegateFlowLayout 프로토콜 채택 후 준수하기

```swift
extension StockRankViewController: UICollectionViewDelegateFlowLayout {
        // Layout 구성 
    func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, sizeForItemAt indexPath: IndexPath) -> CGSize {
        return CGSize(width: collectionView.bounds.width, height: 80)
    }
}
```

## 🍎 collection view cell 구성 및 사용

- 스토리보드에서 cell 클릭후 identity inspector에서 클래스 설정하기
- cell의 identifier 설정하기

## 🍎 collectionView에서 셀 안에서 두 오브젝트 사이에 관계를 맺어줄 때.

- 가로로 배치되어 있는 두 오브젝트 사이에 관계를 맺어줄때는 horizontal spacing으로 제약을 걸어줄수 있다.
- 하나의 오브젝트에 제약을 걸어줄때는 horizontal/vertical spacing이 없다. 
- 즉, 두 오브젝트 사이를 horizontal spacing으로 제약을 걸어준다면, 각각의 제약을 확인했을때 해당 오브젝트 기준으로 제약을 만든다.

## 🍎 indexPath에는 어떤 정보가 들어있나?

- cell의 section과 (row or item) 정보가 들어있다.
- tableViewCell 사용시 indexPath.row
- collectionViewCell 사용시 indexPath.item
