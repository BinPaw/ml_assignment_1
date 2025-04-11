## სახლის ფასების პროგნოზირება
##### Kaggle-ის ამ კონკურსის მიზანია ბინების ფასების პროგნოზირება სხვადასხვა მახასიათებლებზე დაყრდნობით, როგორიცაა კვადრატული მეტრი, ოთახების რაოდენობა და სხვა ქონების დეტალები. მონაცემთა ნაკრები შედგება სხვადასხვა მახასიათებლისგან, რომლებიც საჭიროებენ წინასწარ დამუშავებას: cleaning, feature engineering და feature selection, რომ უფრო კარგად მოერგოს მოდელი და მოგვცეს უფრო ზუსტი შედეგი.

## ჩემი მიდგომა

##### Cleaning: ამოვიღე სტრიქონები რომლებსაც აკლია ფასები და სვეტები, რომლებსაც threshold-ზე მეტი წილი აქვს NaN.
##### Feature Engineering: NaN მნიშვნელობები რიცხვით სტრიქონებში შევავსე მედიანით, რადგან ამან უკეთესი შედეგი მომცა ვიდრე საშუალომ, ხოლო კატეგორიული ცვლადები მოდებით ჩავანაცვლე. რიცხვითი ცვლადები დავასკალირე StandardScaler-ით, კატეგორიული ცვლადები OneHotEncoder-ით გადავიყვანე მოდელისთვის გასაგებ ფორმატში.
##### Feature Selection: SelectKBest-ის და mutual information regression-ის გამოყენებით შევარჩიე მნიშვნელოვანი feature-ები.
##### Training: გამოვიყენე როგორც Ridge Regression, ასევე Linear Regression და ჰიპერპარამეტრები შეირჩა GridSearchCV-ის საშუალებით.
##### მოდელის მუშაობა შევაფასე ლოგარითმულ სკალაში Root Mean Squared Error (RMSE)-ის გამოყენებით.
##### MLflow-ს საშუალებით შევინახე მოდელი საჭირო პარამეტრებთან ერთად.
## რეპოზიტორიის სტრუქტურა
- ##### **model_experiment.ipynb** - სადაც ხდება მონაცემების დამუშავება, მოდელის დატრენინგება და მისი დალოგვა MLflow-ზე.
- ##### **model_inference.ipynb** - სადაც ხდება რომელიმე MLflow-ზე დალოგილი მოდელის ჩამოტვირთვა და მისი მეშვეობით პროგნოზირება სატესტო მონაცემებზე.
- ##### **data** - მონაცემების შემცველი დირექტორია.
## Feature Engineering
##### კატეგორიული ცვლადების რიცხვითში გადაყვანა პირდაპირ ხერხდება OneHotEncoder-ის საშუალებით.
##### NaN მნიშვნელობები რიცხვით სვეტებში შევცვალე მედიანით, კატეგორიულში მოდით.
##### Cleaning გავაკეთე ისეთი სვეტების წაშლით, რომლებსაც რაღაც ზღვარზე მეტი წილი NaN ქონდათ.
## Feature Selection
##### გამოვიყენე SelectKBest, სადაც feature-ების რაოდენობა შევარჩიე ჯერ 5, შემდეგ 20. k=20-მა ბევრად უკეთესი შედეგი დადო 5-თან შედარებით, თუმცა საგრძნობლად გაიზარდა training-ის დრო.
## Training
##### გავტესტე Ridge და Linear Regression მოდელები, Linear Regression ბევრად სწრაფი იყო training-ის დროს, თუმცა Ridge Regression-მა უკეთესი შედეგი დადო.
##### Hyperparameter დავაოპტიმიზირე root mean squared error-ის მინიმალურობის მიხედვით cross validation-ის დროს.
##### საბოლოო მოდელად შევარჩიე Ridge Regression, რადგან ყველაზე კარგი შედეგი მომცა.
## MLflow Tracking
##### [ექსპერიმენტების ბმული](https://dagshub.com/gnada22/ml_assignment_1.mlflow/#/experiments/0?searchFilter=&orderByKey=attributes.start_time&orderByAsc=false&startTime=ALL&lifecycleFilter=Active&modelVersionFilter=All+Runs&datasetsFilter=W10%3D)
### ჩაწერილი მეტრიკები
##### **val_rmse_log** - root mean squared error ლოგარითმულ სკალაში სატესტო მონაცემებზე, რომლებიც სატრენინგო მონაცემების ფაილიდან ამოვიღე შემთხვევითად (20%) და არ მიუღიათ training-ში მონაწილეობა.
##### **best_alpha** - ალფა პარამეტრის ყველაზე ოპტიმალური მნიშვნელობა Ridge Regression-ში
##### **model_type** - რომელი მოდელი იქნა გამოყენებული
### საუკეთესო მოდელის შედეგები
- **rmse**: 0.15722310230490358
- **kaggle score**: 0.15060