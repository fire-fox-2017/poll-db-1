<!-- Release 1  -->

<!-- 1. Hitung jumlah vote untuk Sen. Olympia Snowe yang memiliki id 524. -->

select cm.name, count(v.id) as "Jumlah Voting"
from congress_members cm, votes v
where cm.id = v.politician_id
and cm.id = 524 group by cm.name

<!-- 2. Sekarang lakukan JOIN tanpa menggunakan id `524`. Query kedua tabel votes dan congress_members. -->

select cm.*
from congress_members cm, votes v
where cm.id = v.politician_id
and 
cm.name = "Sen. Olympia Snowe"

<!-- 3. Sekarang gimana dengan representative Erik Paulsen? Berapa banyak vote yang dia dapatkan? -->

select cm.name, count(v.id) as "Jumlah Voting"
from congress_members cm, votes v
where cm.id = v.politician_id
and cm.name = "Rep. Erik Paulsen" group by cm.name

<!-- 4. Buatlah daftar peserta Congress yang mendapatkan vote terbanyak. Jangan sertakan field `created_at` dan `updated_at`. -->

select cm.name, cm.party, cm.location, cm.grade_1996, cm.grade_current, cm.years_in_congress, cm.dw1_score, count(v.id) as "number_of_votes"
from votes v
join congress_members cm
on v.politician_id = cm.id
group by v.politician_id
order by number_of_votes asc
limit 3

<!-- 5. Sekarang buatlah sebuah daftar semua anggota Congress yang setidaknya mendapatkan beberapa vote dalam urutan dari yang paling sedikit. Dan juga jangan sertakan field-field yang memiliki tipe date. -->

select cm.name, cm.party, cm.location, cm.grade_1996, cm.grade_current, cm.years_in_congress, cm.dw1_score, count(v.id) as "number_of_votes"
from votes v
join congress_members cm
on v.politician_id = cm.id
group by v.politician_id
order by number_of_votes desc
limit 3

<!-- Release 2  -->

<!-- 1. Siapa anggota Congress yang mendapatkan vote terbanyak? List nama mereka dan jumlah vote-nya. Siapa saja yang memilih politisi tersebut? List nama mereka, dan jenis kelamin mereka. -->

<!-- 2. Berapa banyak vote yang diterima anggota Congress yang memiliki grade di bawah 9 (gunakan field `grade_current`)? Ambil nama, lokasi, grade_current dan jumlah vote. -->

select v.politician_id, cm.name, cm.location, cm.grade_current, count(v.voter_id) as "total_votes"
from congress_members cm, votes v
where cm.id = v.politician_id
and cm.grade_current < 9
group by v.politician_id
order by grade_current desc 

<!-- 3. Apa saja 10 negara bagian yang memiliki voters terbanyak? List semua orang yang melakukan vote di negara bagian yang paling populer. (Akan menjadi daftar yang panjang, kamu bisa gunakan hasil dari query pertama untuk menyederhanakan query berikut ini.) -->

<!-- 4. List orang-orang yang vote lebih dari dua kali. Harusnya mereka hanya bisa vote untuk posisi Senator dan satu lagi untuk wakil. Wow, kita dapat si tukang curang! Segera laporkan ke KPK!! -->

select v1.id,v1.first_name, v1.last_name, v1.gender, cm.name, count(v2.id) as "jumlah_vote"
from voters v1
left join votes v2 on v1.id = v2.voter_id
left join congress_members cm on cm.id = v2.politician_id
group by v1.id
having jumlah_vote > 1
order by jumlah_vote desc

<!-- 5. Apakah ada orang yang melakukan vote kepada politisi yang sama dua kali? Siapa namanya dan siapa nama politisinya? -->

