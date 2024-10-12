---readme written by Stephen West (2024.08.26)---
## Context
The simulation used in this paper (sim9-1.3) is based on the code that was in Matsumoto & Sasakura (2016), used with permission.
Modifications were made so that the chance of selecting a destination area could be defined per link (defined in link.txt and based on least-cost path analysis).
Additionally, modifications were made to sim.c (590:602) so that an area's carrying capacity would be based on the average skill value of its agents.
Besides these minor structural changes, the simulation program is largely unchanged from its original form.

## Citation
Matsumoto, N and Sasakura, M. 2016.
Cultural and Genetic Transmission in the Jomon–Yayoi Transition Examined in an Agent-Based Demographic Simulation. 
In:. Barceló, JA and Del Castillo, F (eds.). Simulating Prehistoric and Ancient Worlds.
Springer International Publishing. pp. 311–334.

## Directories
- /sim9-1.3 #Directory with files needed to execute the simulation.
- /sim9-1.3/parameters #Directories with the input files and results for each parameter. Move files into sim9-1.3 directory and overwrite to replicate.

## Files
### Input Files
- area.txt (defines simulation areas and settings per area)
    - Columns: area code, area name, carrying capacity, movement rate, over-capacity move rate modifier, over-capacity death rate modifier
- link.txt (defines which areas are linked, chance of agents selecting one area over another)
    - Columns: from area code, to area code, chance of selection by migrating agents.
- BirthRate.txt (defines the birthrates per area, age, & sex)
    -Column 1: area code. Columns 2~129: Annual chance of childbirth for females agents between ages 0 and 127. Columns 130〜257: Annual chance of childbirth for male agents between ages 0 and 127.
- MortalityRate.txt (defines the chance that an agent will die each year per area, age, & sex)
    - Column 1: area code. Columns 2~129: Female mortality rate between ages 0 and 127. Columns 130〜257: Male mortality rate between ages 0 and 127.
- init.txt (defines the initial population, sex ratio, and gvalue of simulation areas)
    - Columms: area code, population distribution type (1 hunter-gatherer, 2 agriculturalist), initial population, male to female ratio, initial gvalue
- Skill.txt (skill value of agents in each areɑ)
    - Column 1: area code. Columns 2~129: Female skill value between ages 0 and 127. Columns 130〜257: Male skill value between ages 0 and 127.

### Output Files
- logN.txt #Log of the simulation output. I have compressed the original log files as they are very large.
- poplog.data #Population totals per year for each simulation area (year, area 1, area 2, ...)
- gvalue.data #Average gvalue per year for each simulation area (year, area 1, area 2, ...)
- skill.data #Average skill value per year for each simulation area (year, area 1, area 2, ...)
- migration.data #Number of migrants leaving area per year for each simulation area (year, area 1, area 2, ...)
- ".png #proc.rb generates graphs of the above values per year for each simulation area.
I have cleaned the output files for each parameter and put them into the Simulation Results folder.

## How to Replicate
A development environment for C, Ruby, and X11 is needed.
1. Move the files from a parameter directory into the sim9-1.3 directory and overwrite.
2. Use the terminal to bash gogo-sim.sh and execute sim. 
	-gogo-sim.sh specifies the settings that are used by the simulation (timeframe, marriage rules, etc.)
	-The results are written in logN.txt.
3. Use the terminal to bash gogo-graph.sh and organize / graph the data.
	-The relevant outputs are: poplog.data, gvalue.data, skill.data
	-These are the files I used to produce the figures that are used in the publication.
	-The graphs display the results per area and are were not used in the publication.
The ABM is stochastic so replicated results may vary slightly from the original data.

## How to Modify
- Input files can be easily modified to model new population values, carrying capacities, etc.
    - Changing the number of areas requires changes to areas.txt, init.txt, link.txt, and modifying the ruby scripts.
- Simulation settings (options) can be modified via the command given in shell script (gogo-sim.sh)
    - Refer to the original documentation below for information on simulation options.
- The variable carrying capacity functionality I added in sim.c (590:602) is hardcoded and must be manually changed.
- If changes to the C script are made, sim must be rebuilt with the makefile provided for the changes to take effect.

Refer to the original documentation for further details. A development environment for C, Ruby, and X11 is needed.

## Documentation
Below is the original documentation that was provided by Matsumoto and Sasakura (in Japanese). 
------------------------------------------------------------------------------

---readme.text---

実行の仕方

simがある一つ上のディレクトリに

hinapop.plot
hinaskill.plot
proc.rb
skill.rb
batch?.sh

をおいて、simおよび入力用.txtのあるディレクトリに入って、

../batch?.sh

とする。

=====
ファイル解説
hinapop.plot : 人口推移をgnuplotでグラフ化するためのplotファイル
hinaskill.plot : skill平均推移をgnuplotでグラフ化するためのplotファイル
proc.rb : 人口推移のデータ収集スクリプト
skill.rb : skill平均推移のデータ収集スクリプト
batch?.sh : 実行バッチファイル(どのような実行をしたかを記録するため必ずバッチファイルにて実行のこと)

marriagestatistical.rb　: 初婚年齢，こどもの数などの情報をグラフ化するためのスクリプト(でも実際にはこれは使ってない)
firstmarriage.rb : 初婚年齢データ収集スクリプト
childnum.rb : 生涯で生んだこどもの数のデータ収集スクリプト
nochilddead.rb : 結婚してかつこどものない人のデータ収集スクリプト
nomarriagedead.rb : 結婚経験のない人のデータ収集スクリプト
firstmarriage.plot : 初婚年齢データグラフひながた
childnum.plot : 生涯で生んだこどもの数のデータグラフひながた
nochilddead.plot : 結婚してかつこどものない人のデータグラフひながた
nomarriagedead.plot : 結婚経験のない人のデータグラフひながた
makegraph?.sh : 初婚年齢，こどもの数などの情報のグラフ化を実行するバッチファイル
------------------------------------------------------------------------------
------------------------------------------------------------------------------
---HowtoTest.text---
実験データの取りかたまとめ 

入力ファイルの場所（テストでこれまで使っているもの）
	area5-input/

データ収集用バッチファイル
	sim-script/

グラフのpngファイルをだすバッチファイルを動かす時にはX11のterminalから出ないと出ない．
だからXQuarts を立ち上げてそのterminalから実行すること．

必要なのは
6つの *.txt
proc.rb
pop.plot
sim
gogo.sh
-------------------------------------------------------------------------------
-------------------------------------------------------------------------------
---HowtoUse.text---

#
# ReadMe - Simulation of population moving
#              $Id: ReadMe,v 1.4 2013/02/24 17:12:26 void Exp $
# vi: ts=8 sw=8 sts=8

起動方法
	./sim [-b_d._d] [-d_n] [-f_n] [-i_n] [-m] [-r_n] [-w_d._d] years
	yearsにはシミュレイション期間を指定します(単位: 年)。
	例:
		./sim -t 2000
	期間を省略した場合は1000年となります。

オプション
	-b_d_._d	出産による母親の死亡率 (at Birth) [0.01]
	-d_n	初期人口年代分布多産多死型(1:ピラミッド型)縄文型 or 2:安定型(ベル型)(弥生型) (Distribution type) [1]
	-f_n	一緒に移動する家族の範囲指定 (2 or 3親等) (Family) [3]
	-i_n	結婚相手タブー親等指定 (デフォルト3親等) (Incest) [3]
	-m	結婚アルゴリズムの選択。女性の結婚可能年齢を45歳まで(年齢差を考慮しない)にする。
		指定しない場合は最大年齢なし(年齢差のみ考慮)。 (Marrage Type) [false]
	-r_n	移動先決定の範囲に関する家族の範囲指定(_n親等)。 (Relation) [3]
	-w_d_._d	家族での移動発生率。 (With) [0.5]
	-a	人口の出力を年齢層(10年区切り)ごとにします。debug用です。 (Age layer) [false]
	-g	人口分布のグラフを表示します。debug用です。 (Graph) [false]
	-k	疑似乱数の種まきをしません。つねに同じ結果になります。debug用です。 (Keep) [false]
	-l	途中経過を表示せず、最初と最後の結果のみを表示します。 (Last only) [false]
	-p_n	近い親戚のない死亡者のデータを整理するタイミング(default 10年ごと、0を指定すると行なわない)。 (Purge) [10]
	-t	死亡ごとにその人の情報を表示します。 (Tombstone) [false]
	-x	debug用です (Examine) [false]

       -x2 debug用
      　　　女性が亡くなった時にその女性の
    　　　　結婚経験の有無，結婚経験有りの場合初婚年齢，生んだこどもの数
    　　　をstdoutに表示．
　　　       シミュレーション終了時，生きている人の情報は入れない．

<結婚関係(sim9-1以降)>
	-A     結婚相手を自分の地域だけでなく近隣地域からも選ぶ(sim9-1仕様)

  <gvalueによる出産率調整>
        -Bf_d_._d 男性のgvalueが0.5以上の時，出産率を d.d倍する
        -Bm_d_._d 女性のgvalueが0.5以上の時，出産率を d.d倍する
        -Bb_d_._d 両親のgvalueが0.5以上の時，出産率を d.d倍する
        -Be_d_._d 片親のgvalueが0.5以上の時，出産率を d.d倍する

  <gvalueによる移動調整>
  　　　　移動の場合，もし今いるところと移動先のgvalueの違いの絶対値が
        -Md_d_._d　d.d以上であったら
        -Ms_d_._d　移動確率をd.d倍する．

  <gvalueによる結婚相手調整>
        結婚相手選択の時，配偶者候補とのgvalueの差の絶対値が
        -Wd_d_._d　d.d以下ならば必ずこの相手と結婚する
        -Wp_d_._d　そうでない場合の結婚確率をここで設定したd.d倍にする．

  <gvalueによる遺伝モデル選択> (sim8-2以降)
  	-Go gvalue遺伝に aone-locus two-allele model を使う
　　　　　(何もつけないときは，両親の値の半分をgvalueとする)
  		
 <Skill関係オプション>
	-Si_n	n歳で最初の師匠を捜す[15]
	-Sx_n	_n歳ごとに
	-Sa_d_._d	技量パラメーターを_d_._d倍する
	-Sy_n	_n歳以上では
	-Sz_n	_n歳ごとに
	-Sb_d_._d	_d_._d倍する
	-SX_n	_n歳ごとに「師匠」を一人さがす
	-Sc_d_._d	i)「師匠」の技量パラメタの_d_._d倍の値を自分の技量パラメタに上乗せする
	-Sd_d_._d	ii) 自分の技量パラメタを「師匠」の技量パラメタの_d_._d倍にする
	-Ss	iを使う場合はfalse、iiを使う場合はtrue

       -Se があったら，sim7-6の内容に，なければsim7-5そのままに．
       -SEn で師匠選択方法の選択(sim7-6)
            n=1 ランダム
            n=2 3親等以内
            n=3 地域で一番
	    n=4 両親のうち生きている方からランダム(今は両方死んでたら0にはしてない)
	    n=5 同じ地域の生きている親戚(?親等以内)のうちからランダム(全員死んでたら0)
         nがそれ以外の場合(デフォルト)はランダムで．  
       -Skn -SE2の時，その親等数をnで指定可能(デフォルト3)
       -Sm 正規分布の平均を指定(sim7-6) 指定しなかったら10.0
       -Sg 正規分布の標準偏差(σ)を指定(sim7-6) 指定しなかったら1.0
       -Sp sim7-6 ランダムの時のリストたどりをprevからにする(つけないとnextから)
       -Sun sim7-6 スキル上昇年齢．n歳の時に(一度きり) (デフォルトは40)

    <一夫多妻オプション>
　　　-Pm  これがあったら一夫多妻制．なければ一夫一婦制 .plural_marriage = true[false]
　　　-Px_n  これがあったら男性の最大配偶者数は固定でその値がn
            .plural_max = n [1]
     -Pb_d_._d これがあったら男性の最大配偶者数は線形式で，スキルが_d_._d人毎に1人増える．
　　　　　　　　　-Px_n と -Pb_d_._d の両方が設定されていたら，それにより計算される最大配偶者数が大きい方が有効
            .plural_unit = d.d [0.0]
　　　-Ps  これがあったら配偶者選択アルゴリズムは文化スキルの大きい方．なければこれまで通り最初に見つかった人．
           .plural_ma = 1 [0]
             0:これまで通り最初に見つかった人
	     1:文化スキルの大きい方



ファイル
	area.txt	地域の属性を設定します。
				1行に1地域分です。タブ区切りです。
			例:
				10      地域1  10000   0.5
			1. 地域コード
			2. 地域名
			3. 人口保持キャパシティ
			4. 人口移動率
			5. キャパシティを越えた時の移動率倍率(この数倍だけ多く移動する)
			6. キャパシティを越えた時の死亡率倍率(この数倍だけ多く死亡する)
			7. クライシス状態(キャパシティを越えて移動率，死亡率が多くなった状態)から通常状態へ戻るきっかけ．キャパシティのこの値倍の人口を切ったら通常状態に戻る
			8. sim7-5 version 以降必要．師匠を後ろからさがすか前から探すか(p or n) sim7-6では意味無し
			9. sim7-5 version 以降必要．最初に見つけた候補を師匠とするかそれともn番目(nの値はランダム)に見つけた候補を師匠とするか(c or r) sim7-6では意味無し
			10. sim7-5 version 以降必要．前に師匠とした人がいたらそれを優先するか否か(r or n) sim7-6では意味無し
			
	link.txt	地域の隣接関係を設定します。
				1から始まる地区コードのペアをタブで区切って行ごとに書いてください。
					1	2
					1	3
					2	1
					...
				地域1と地域2が双方向につながっている場合、1	2と2 1の両方が必要です

	BirthRate.txt	出生率を設定します。
					1行に1地域分で、1行の中にタブ区切りで年齢別、男女別です。
			1. 地域コード
			2 〜 129. 0〜127歳までの女性が『女児』を出産する確率
			130 〜 257. 〜127歳までの女性が『男児』を出産する確率
			例:
			18  0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.3 0.3 0.4 0.4 0.5 0.5 0.6 0.6 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.4 0.4 0.3 0.3 0.2 0.2 0.1 0.1 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.3 0.3 0.4 0.4 0.5 0.5 0.6 0.6 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.4 0.4 0.3 0.3 0.2 0.2 0.1 0.1 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0

	MortalityRate.txt	死亡率(前年まで生きてた人がその年に死ぬ確率)を設定します。
			1. 地域コード
			2 〜 129. 0〜127歳までの女性の死亡率
			130 〜 257. 〜127歳までの男性の死亡率

	init.txt	地域ごと初期の人口分布等を設定します。
			タブ区切りで地域コード、分布タイプ、初期人口、男女比率, gvalue初期値の順に指定します。
			(男女比率は女性の人数に対する男性の人数の比率です)
			例:
			11      1       1000    1.5	1.0

	Skill.txt      初期人口のスキル初期値および生まれたときの初期値の設定
			1. 地域コード
			2 〜 129. 0〜127歳までの女性の死亡率
			130 〜 257. 〜127歳までの男性の死亡率
		ただし，2 が n となっていたら初期人口のスキルは正規分布で生成．生まれた時の初期値は0. この場合，3以降は書かなくていい．

出力
	1年ごと、地区ごとにカンマ区切りの1行が出力されます
	例:
	沖縄県,21,15,11,19,4,15,11,16,12,13,9,18,8,19,9,19,15,15,13,13,10,18,7,10,11,6,9,17,6,17,7,17,5,11,8,17,9,5,12,11,8,18,4,12,6,13,6,11,8,5,6,9,5,9,7,8,7,3,4,8,6,11,2,5,13,12,5,7,5,7,4,5,4,6,8,9,8,4,2,10,3,3,5,9,5,10,2,5,7,2,4,7,3,10,4,9,3,4,5,3,4,3,3,4,2,2,1,3,0,8,0,3,3,5,5,4,2,3,2,3,1,2,2,3,3,0,2,2,2,1,1,2,2,4,4,1,1,0,1,3,1,1,1,2,1,1,1,0,0,0,2,1,0,1,0,2,0,0,0,1,0,0,1,0,1,0,0,0,0,0,0,1,1,0,0,0,0,1,0,0,1,1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1000,1.46,0,-0
	・地区名
	・年齢別人口(0歳以上、1歳未満、女)
	・年齢別人口(0歳以上、1歳未満、男)
	・年齢別人口(1歳以上、2歳未満、女)
	・年齢別人口(1歳以上、2歳未満、男)
	・年齢別人口(2歳以上、3歳未満、女)
	・年齢別人口(2歳以上、3歳未満、男)
			...
	・年齢別人口(126歳以上、128歳未満、女)
	・年齢別人口(126歳以上、128歳未満、男)
	・その地域の総人口
	・男女比(女性の人数に対する男性の比率です。ただし女性が0人のときは0となります)
	・前年からの自然増人口(その地域で生まれた人の数)
	・前年からの自然減人口(その地域で死亡した人の数)
	・流入/流出地域名1
	・前年からの流入者数(流入元地域1)
	・前年からの流出者数(入出先地域1)
	・流入/流出地域名2
	・前年からの流入者数(流入元地域2)
	・前年からの流出者数(入出先地域2)
	・流入/流出地域名3
	・前年からの流入者数(流入元地域3)
	・前年からの流出者数(入出先地域3)
			...
---
written by Kusakabe Youichi (2010/11/1)
-------------------------------------------------------------------------------
