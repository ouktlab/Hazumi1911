# Hazumi1911
Osaka University Multimodal Dialogue Corpus (Hazumi1911)

Hazumi1911では以下のファイル群を公開している．
```
1. ビデオデータ
2. MS Kinectデータ
3. 閲覧用ELANファイル
4. 実験用ダンプファイル
5. アンケートデータ
6. 生体信号データ
```
このうち，ここでは3.と4.と5.と6.を公開している．
それぞれ，elan/，dumpfiles/，questionnaire/，biosensor/にある．

1.と2.は，データ利用に関する契約を交わした利用者のみに対して，NII IDR
（国立情報学研究所 情報学研究データリポジトリ）から配布される．
データの入手方法や概要説明ドキュメントは，NII IDRサイトを参照のこと．  
https://www.nii.ac.jp/dsc/idr/rdata/Hazumi/  
https://www.nii.ac.jp/dsc/idr/rdata/Hazumi/documents/HazumiOverviewInPerson.pdf

## 3. 閲覧用ELANファイル
アノテーションや書き起こしを全て含んだeaf（ELAN annotation fomat）ファイ
ルである．
アノテーションツールELANで，実験参加者ビデオを読み込んで使用する．

elan/ 以下に，(実験参加者ID).eaf という名前で置かれている．実験参加者ID
は以下で示すYYMMGAANNの9文字である．  
　記号 | 説明
 ------------ | -------------
　YYMM	| コーパスのバージョン．ここでは1911である．  
　G | 実験参加者の性別．M（男性）もしくはF（女性）．  
　AA | 実験参加者の年代．20から70まで．  
　NN | 上記7文字と合わせてIDとなるように付番．
 
例えば1911F2001は「Hazumi1911の，20歳代のある女性のデータ」を意味する．

交換と呼ばれる単位ごとに，以下で示されるデータが付与されている．

#### 実験参加者の発話の書き起こし
#### システム発話とその対話行為
.eafファイルには11種類の対話行為ラベルが付与されている．ただし実験用ダンプファイルにおいて1-hot化されているのは，Hazumi1902と同様の8種類のみである．
詳しくは[概要説明ドキュメント](https://www.nii.ac.jp/dsc/idr/rdata/Hazumi/documents/HazumiOverviewInPerson.pdf)を参照のこと．

#### 心象アノテーション
複数の第三者が付与したもの (TS: Third Sentiment) と，実験参加者本人が付与したもの（振り返りアノテーション） (SS: Self Sentiment) がある．
#### 話題継続アノテーション


## 4. 実験用ダンプファイル
対話中のユーザ（実験参加者）の心象を含むラベルデータを予測するためのマルチモーダル機械学習実験を行うための利用を想定している．このファイルは， 機械学習の入力として，ユーザの音声，映像，発話内容（言語）から抽出したマルチモーダル特徴量（実数値），機械学習の出力として心象（本人・第3者）・話題継続アノテーションの値（連続値とカテゴリ値）を格納している．

実験用ダンプファイルは以下の論文で使用されたものに言語特徴量を加えたものである．生体信号特徴量は，以下の研究で使われたものの一部のみを含む．  
```
Shun Katada, Shogo Okada, Yuki Hirano, and Kazunori Komatani. 2020. Is She Truly Enjoying the Conversation? Analysis of Physiological Signals toward Adaptive Dialogue Systems. In Proceedings of the 2020 International Conference on Multimodal Interaction (ICMI '20). Association for Computing Machinery, New York, NY, USA, 315–323. DOI:https://doi.org/10.1145/3382507.3418844
```

### フォルダの構造とファイル：
dumpfiles/      
　├ 1911F2001.csv　  
　├ 1911F2002.csv　  
　:  
　:  

各ファイル（例：1911F2001.csv）は各セッションにおける参加者の発話対ごとの
マルチモーダル特徴量とアノテーションの値を含んでいる．
1911F7001, 1911M3001, 1911M3002, 1911M5003の4名分については，OpenFaceの結果に欠損が生じていたため，
ダンプファイルは存在しない． この結果，計26人分のダンプファイルを格納している．

### 実験用ダンプファイル中のデータ説明：
各列はデータの属性，各行は1発話対で抽出された各特徴量の数値を格納している．  
1行目はデータの属性名を示す．


#### start(exchange), end(system), end(exchange):
ユーザの上半身動画（mp4）におけるシステム, ユーザの発話開始, 終了時間 

#### kinectstart(exchange), kinectend(system), kinectend(exchange): 
kinectデータにおけるシステム, ユーザの発話開始, 終了時間 (kinect)

#### SS_ternary , TC_ternary, TS_ternary : 
self-sentiment（ユーザが付与した自身の心象ラベル）， topic continuance（話題継続ラベル），third sentiment（第3者が付与したユーザ心象）のアノテーションの値（SS, TC, TS）を，三段階でラベル化したもの．2が高群, 1がニュートラル, 0が低群．

##### 3段階のラベル化方法：
self-sentimentの場合，5点以上を高群，4点をニュートラル，3点以下を低群と決定．  
Topic continuanceとThird sentimentの場合アノテータの平均値が4.5より高ければ高群, 3.5より低ければ低群, それ以外は中間群と決定. 

#### SS：
ユーザ本人により付与されたself-sentimentのアノテーション値. 

#### TC1~TC5:
5名により付与されたtopic continuanceのアノテーション値.

#### TS1~TS5:
5名により付与されたthird sentimentのアノテーション値．

#### pcm_RMSenergy_sma_max ~ F0_sma_de_kurtosis: 
OpenSmileによって抽出された韻律特徴量. Config fileはIS09_emotion.conf.

#### word#001~su： 
ユーザとシステムの発話から抽出された言語特徴量.
書き起こしデータ内のユーザ発話から特徴量を抽出した．日本語形態素解析器Mecabを使用して形態素解析を行い，1発話内に含まれる品詞ごとの単語数および各単語の出現回数を表すBag-of-Words（BoW）を言語特徴量として使用した．
またシステム発話のタイプといった特徴量も含まれる．

#### 17_acceleration_max~AU45_c_mean: 
顔表情特徴量. 「17」のような数字はOpenFaceのlandmarkを示す. AUはaction unit.
landmark特徴量はOpenFaceから得られる2次元座標データ(30fps)をもとに目の周り, 口の周りなどの12点のフレーム間速度, 加速度をexchange区間ごとに求め最大値, 平均値, 標準偏差を特徴量とした. またexchange区間のAUの有無の平均を特徴量とした. 

#### HandLeft_acceleration_max~duration: 
動作特徴量. Durationは発話時間
Kinectの3次元間座標データ(30fps)をもとに頭部, 肩などの部位のフレーム間速度, 加速度を求め, exchange区間ごとに最大値, 最小値, 平均値を特徴量とした. 

#### EDA_mean, EDA_std, HR_mean, HR_std:
生体特徴量. 
実験参加者の手首にE4 wristbandを装着し，単位時間当たりの心拍（回数）と皮膚電位（μSiemens）を測定した．ここでは発話交換内での皮膚電位と心拍の平均値および標準偏差を含めた．


### マルチモーダルデータの同期方法：
ユーザの音声データ・姿勢データはKinect V2センサ (Kinect)より，RGB画像データはビデオカメラより取得した．
Kinectのデータ記録時間とビデオカメラのデータ記録時間において，各セッションのエージェント発話開始点を特定して遅延を計算し，両デバイスから得られたデータの時刻同期を行った．またMMDエージェントの発話ログファイルを用いて，
ユーザの発話区間・エージェントの発話区間の切り分けを行った．
言語特徴量（素性）は発話区間に対応するユーザ発話の書き起こしテキストから抽出した．
生体信号データの各csvファイルにはUnix時間が記載されており，この時刻はKinectの時刻に対応している．
上記の手順で，音声・言語・画像の情報の時間同期を行った．  
詳しくは[概要説明ドキュメント](https://www.nii.ac.jp/dsc/idr/rdata/Hazumi/documents/HazumiOverviewInPerson.pdf)を参照のこと．

## 5. アンケートデータ
実験開始前と実験終了後の両方に実施したアンケートに関するデータである．

questionnaire/  
　├ 1911questionnaire-items.pdf　本人とWizardによるアンケートで使用した質問紙  
　├ 1911questionnaire.xlsx　本人とWizardによるアンケート結果  
　├ 1911questionnaire-3rdparty-rapport.xlsx　第3者アノテータ5名が付与したラポール18項目
　├ 1911questionnaire-3rdparty-personality.xlsx　第3者アノテータ5名が付与した性格特性
　├ questionnaire-items-3rdparty-rapport.pdf　第3者アノテーションでのラポール18項目の質問紙  
　└ questionnaire-items-3rdparty-personality.pdf　第3者アノテーションの性格特性の質問紙  

本人とWizardによるアンケート結果ファイルには，まずHazumi1902と同様に，実験参加者（実験前），実験参加者（実験後），
Wizard（実験前），Wizard（実験後）の4つのタブがある．このそれぞれにおい
て，実験参加者は18項目，Wizardは簡略化した3項目に対して，8段階で回答した
結果が記録されている．これに加え，「記述式」「性格特性」という2つのタブが
Hazumi1911では追加されている．
詳しくは[概要説明ドキュメント](https://www.nii.ac.jp/dsc/idr/rdata/Hazumi/documents/HazumiOverviewInPerson.pdf)を参照のこと．

さらに2023年9月に，第3者アノテータ5名が，それぞれビデオを見て事後的に
付与したアンケート結果（ラポール18項目，性格特性）ファイルを追加した．
それぞれのアノテーションで用いた質問紙とともに置いている．


## 6. 生体信号データ
Empatica社製のE4 wristbandを用いて取得した対話中の実験参加者の生体信号データである．
biosensorフォルダには各セッションにおいて収集された生体信号データセットが以下のようにzipファイルで
格納されている．

biosensor/  
　├ 1911F2001.zip　  
　├ 1911F2002.zip   
　:  
　:  

各zipファイルを解凍すると心拍，皮膚電気活動といった生体信号データが格納されている．具体的には，
各データごとのcsvファイルと，各データの説明を記述したファイル（info）が以下のように格納されている．

1911F2001/  
　├ info.txt 各データの説明  
　├ EDA.csv　皮膚電位データ  
　├ BVP.csv  容積脈波データ  
　├ HR.csv  心拍データ  
　├ TEMP.csv  皮膚温度データ  
　└ ACC.csv  加速度データ  
  
詳しくは[概要説明ドキュメント](https://www.nii.ac.jp/dsc/idr/rdata/Hazumi/documents/HazumiOverviewInPerson.pdf)を参照のこと．

# Authors
* 駒谷 和範（大阪大学 産業科学研究所） komatani@sanken.osaka-u.ac.jp
* 岡田 将吾（北陸先端科学技術大学院大学） okada-s@jaist.ac.jp
