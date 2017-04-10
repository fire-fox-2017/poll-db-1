<!-- Release 1  -->

<!-- 1. Hitung jumlah vote untuk Sen. Olympia Snowe yang memiliki id 524. -->
sqlite> select count(id) from votes where politician_id = 524;
22
<!-- 2. Sekarang lakukan JOIN tanpa menggunakan id `524`. Query kedua tabel votes dan congress_members. -->
sqlite> select * from votes v left join congress_members c on v.politician_id = c.id
<!-- 3. Sekarang gimana dengan representative Erik Paulsen? Berapa banyak vote yang dia dapatkan? -->
sqlite> select count(v.id) from votes v left join congress_members c on v.politician_id = c.id where c.name="Rep. Erik Paulsen";
21
<!-- 4. Buatlah daftar peserta Congress yang mendapatkan vote terbanyak. Jangan sertakan field `created_at` dan `updated_at`. -->
sqlite> select c.id, c.name, c.party, c.location, c.grade_1996, c.grade_current, c.years_in_congress, c.dw1_score, count(*) from votes v left join congress_members c on v.politician_id = c.id group by c.id order by count(*) desc limit 3 ;
<!-- 5. Sekarang buatlah sebuah daftar semua anggota Congress yang setidaknya mendapatkan beberapa vote dalam urutan dari yang paling sedikit. Dan juga jangan sertakan field-field yang memiliki tipe date. -->
select c.id, c.name, c.party, c.location, c.grade_1996, c.grade_current, c.years_in_congress, c.dw1_score, count(*) from votes v left join congress_members c on v.politician_id = c.id group by c.id order by count(*) asc limit 3 ;
<!-- Release 2  -->

<!-- 1. Siapa anggota Congress yang mendapatkan vote terbanyak? List nama mereka dan jumlah vote-nya. Siapa saja yang memilih politisi tersebut? List nama mereka, dan jenis kelamin mereka. -->
select first_name, last_name, gender from voters where id in (select voter_id from votes where politician_id = (select politician_id from votes group by politician_id order by count(*) desc limit 1));
<!-- 2. Berapa banyak vote yang diterima anggota Congress yang memiliki grade di bawah 9 (gunakan field `grade_current`)? Ambil nama, lokasi, grade_current dan jumlah vote. -->
select c.name, c.location, grade_current, votes_count.total_votes from congress_members c inner join (select politician_id, count(*) as total_votes from votes where politician_id in (select id from congress_members where grade_current < 9) group by politician_id) as votes_count on c.id = votes_count.politician_id;

<!-- 3. Apa saja 10 negara bagian yang memiliki voters terbanyak? List semua orang yang melakukan vote di negara bagian yang paling populer. (Akan menjadi daftar yang panjang, kamu bisa gunakan hasil dari query pertama untuk menyederhanakan query berikut ini.) -->
select c.id, c.location, table_b.total_votes from congress_members c inner join (select politician_id, count(*) as total_votes from votes v group by politician_id order by count(*) desc limit 10) as table_b on c.id = table_b.politician_id;

<!-- 4. List orang-orang yang vote lebih dari dua kali. Harusnya mereka hanya bisa vote untuk posisi Senator dan satu lagi untuk wakil. Wow, kita dapat si tukang curang! Segera laporkan ke KPK!! -->
select table_1.first_name, table_1.last_name, table_1.gender, table_2.politician_id, table_1.congress_member_name, table_2.total_votes from
(select table_b.first_name as first_name, table_b.last_name as last_name, table_b.gender as gender , congress_members.name as congress_member_name, congress_members.location as location
from
(select voters.first_name, voters.last_name, voters.gender, votes.politician_id as politician_id from voters inner join votes on voters.id = votes.id where voters.id in (select voter_id from votes where politician_id in (select politician_id from votes group by politician_id order by count(*) desc limit 10))) as table_b
inner join congress_members on congress_members.id = table_b.politician_id) as table_1
inner join
(select congress_members.id as politician_id, congress_members.location as location, table_b.total_votes as total_votes from congress_members inner join (select politician_id, count(*) as total_votes from votes group by politician_id order by count(*) desc limit 10) as table_b on congress_members.id = table_b.politician_id
) as table_2
on table_1.location = table_2.location;

<!-- 5. Apakah ada orang yang melakukan vote kepada politisi yang sama dua kali? Siapa namanya dan siapa nama politisinya? -->
