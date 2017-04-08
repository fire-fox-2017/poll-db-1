<!-- Release 1  -->

<!-- 1. Hitung jumlah vote untuk Sen. Olympia Snowe yang memiliki id 524.
  Jawab : select count(*) from votes where politician_id=524;
 -->

<!-- 2. Sekarang lakukan JOIN tanpa menggunakan id `524`. Query kedua tabel votes dan congress_members.
  Jawab :select * from congress_members c join votes v on c.id = v.politician_id where c.name = "Sen. Olympia Snowe";
-->

<!-- 3. Sekarang gimana dengan representative Erik Paulsen? Berapa banyak vote yang dia dapatkan?
Jawab : select count(*) from congress_members c join votes v on c.id = v.politician_id where c.name = "Rep. Erik Paulsen" -->

<!-- 4. Buatlah daftar peserta Congress yang mendapatkan vote terbanyak. Jangan sertakan field `created_at` dan `updated_at`.
Jawab : select c.id,c.name,c.party,c.location,c.grade_1996,c.grade_current,c.years_in_congress,c.dw1_score,count(v.id)number_of_votes from congress_members c join votes v on c.id=v.politician_id group by c.id order by number_of_votes desc limit 3
 -->

<!-- 5. Sekarang buatlah sebuah daftar semua anggota Congress yang setidaknya mendapatkan beberapa vote dalam urutan dari yang paling sedikit. Dan juga jangan sertakan field-field yang memiliki tipe date.
Jawab :select c.id,c.name,c.party,c.location,c.grade_1996,c.grade_current,c.years_in_congress,c.dw1_score,count(v.id)number_of_votes from congress_members c join votes v on c.id=v.politician_id group by c.id order by number_of_votes asc
 -->

<!-- Release 2  -->

<!-- 1. Siapa anggota Congress yang mendapatkan vote terbanyak? List nama mereka dan jumlah vote-nya. Siapa saja yang memilih politisi tersebut? List nama mereka, dan jenis kelamin mereka.
Jawab : select v.first_name||' '|| v.last_name as nama , v.gender as jenis_kelamin , vo.politician_id from voters v join votes vo on v.id=vo.voter_id order by vo.politician_id
-->

<!-- 2. Berapa banyak vote yang diterima anggota Congress yang memiliki grade di bawah 9 (gunakan field `grade_current`)? Ambil nama, lokasi, grade_current dan jumlah vote.
Jawab : select c.name,c.location,c.grade_current,count(v.id) as number_of_votes from congress_members c join votes v on c.id=v.politician_id where c.grade_current<9 group by c.id  -->

<!-- 3. Apa saja 10 negara bagian yang memiliki voters terbanyak? List semua orang yang melakukan vote di negara bagian yang paling populer. (Akan menjadi daftar yang panjang, kamu bisa gunakan hasil dari query pertama untuk menyederhanakan query berikut ini.)
JAWAB : select B.first_name||' '||B.last_name as voter ,B.gender ,A.pid,name ,A.location,MAX(A.number_of_votes) total_vote FROM (
select c.name as name,c.location as location,count(v.id) as number_of_votes ,v.politician_id as pid, v.voter_id as vid from congress_members c  join votes v on c.id=v.politician_id  group by c.id ) A , voters B
-->

<!-- 4. List orang-orang yang vote lebih dari dua kali. Harusnya mereka hanya bisa vote untuk posisi Senator dan satu lagi untuk wakil. Wow, kita dapat si tukang curang! Segera laporkan ke KPK!!
JAWAB :select nama,jenis_kelamin,total from(
select a.first_name||' '||a.last_name as nama,a.gender as jenis_kelamin,c.name,count(b.politician_id) as total from voters a, votes b,congress_members c where a.id=b.voter_id AND c.id=b.politician_id  group by b.voter_id order by total desc) where total>1
-->

<!-- 5. Apakah ada orang yang melakukan vote kepada politisi yang sama dua kali? Siapa namanya dan siapa nama politisinya? -->
