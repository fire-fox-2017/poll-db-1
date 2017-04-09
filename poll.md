<!-- Release 1  -->

<!-- 1. Hitung jumlah vote untuk Sen. Olympia Snowe yang memiliki id 524. -->
select count(*) from votes where politician_id = 524;
<!-- 2. Sekarang lakukan JOIN tanpa menggunakan id `524`. Query kedua tabel votes dan congress_members. -->
select congress_members.* from votes join congress_members on votes.politician_id = congress_members.id and congress_members.name = "Sen. Olympia Snowe";

<!-- 3. Sekarang gimana dengan representative Erik Paulsen? Berapa banyak vote yang dia dapatkan? -->
select count(*) from votes join congress_members on votes.politician_id = congress_members.id and congress_members.name = "Rep. Erik Paulsen";

<!-- 4. Buatlah 3 daftar peserta Congress yang mendapatkan vote terbanyak. Jangan sertakan field `created_at` dan `updated_at`. -->
select cm.name, cm.party, cm.location,cm.grade_1996, cm.grade_current, cm.years_in_congress, cm.dw1_score, count(v.id) as number_of_votes from votes v join congress_members cm on v.politician_id = cm.id  group by v. politician_id order by count(v.id) desc limit 3;

<!-- 5. Sekarang buatlah sebuah daftar semua anggota Congress yang setidaknya mendapatkan beberapa vote dalam urutan dari yang paling sedikit. Dan juga jangan sertakan field-field yang memiliki tipe date. -->
select cm.name, cm.party, cm.location,cm.grade_1996, cm.grade_current, cm.years_in_congress, cm.dw1_score, count(v.id) as number_of_votes from votes v join congress_members cm on v.politician_id = cm.id  group by v. politician_id order by count(v.id) asc limit 3;

<!-- Release 2  -->

<!-- 1. Siapa anggota Congress yang mendapatkan vote terbanyak? List nama mereka dan jumlah vote-nya. Siapa saja yang memilih politisi tersebut? List nama mereka, dan jenis kelamin mereka. -->
select first_name, last_name, c.politician_id as politician_id, c.name as name , c.total_vote as total_vote from (select voter_id, b.* from votes left join (select a.*, cm.name as name from (select politician_id, count(id) as total_vote from votes group by politician_id) a left join congress_members cm on a.politician_id = cm.id) b on votes.politician_id = b.politician_id)c left join voters on c.voter_id = voters.id order by politician_id;

<!-- 2. Berapa banyak vote yang diterima anggota Congress yang memiliki grade di bawah 9 (gunakan field `grade_current`)? Ambil nama, lokasi, grade_current dan jumlah vote. -->
select a.*, count(votes.id) as total_vote from (select id as politician_id, name, location, grade_current from congress_members where grade_current < 9) a join votes on a.politician_id = votes.politician_id group by a.politician_id order by a.grade_current desc;

<!-- 3. Apa saja 10 negara bagian yang memiliki voters terbanyak? List semua orang yang melakukan vote di negara bagian yang paling populer. (Akan menjadi daftar yang panjang, kamu bisa gunakan hasil dari query pertama untuk menyederhanakan query berikut ini.) -->

<!-- 4. List orang-orang yang vote lebih dari dua kali. Harusnya mereka hanya bisa vote untuk posisi Senator dan satu lagi untuk wakil. Wow, kita dapat si tukang curang! Segera laporkan ke KPK!! -->

<!-- 5. Apakah ada orang yang melakukan vote kepada politisi yang sama dua kali? Siapa namanya dan siapa nama politisinya? -->
