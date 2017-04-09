<!-- Release 1  -->

<!-- 1. Hitung jumlah vote untuk Sen. Olympia Snowe yang memiliki id 524. -->
<!-- SELECT COUNT(*) FROM votes WHERE politician_id = 524; -->

<!-- 2. Sekarang lakukan JOIN tanpa menggunakan id `524`. Query kedua tabel votes dan congress_members. -->
<!-- SELECT * FROM votes JOIN congress_members ON votes.politician_id = congress_members.id WHERE congress_members.id = 524 -->

<!-- 3. Sekarang gimana dengan representative Erik Paulsen? Berapa banyak vote yang dia dapatkan? -->
<!-- select count(*) from votes join congress_members on votes.politician_id = congress_members.id where congress_members.name = 'Rep. Erik Paulsen'; -->

<!-- 4. Buatlah daftar peserta Congress yang mendapatkan vote terbanyak. Jangan sertakan field `created_at` dan `updated_at`. -->
<!-- select congress_members.id,name,party,location,grade_1996,grade_current,years_in_congress,dw1_score,count(*) as number_of_votes from votes join congress_members on votes.politician_id = congress_members.id group by votes.politician_id order by count(*) desc limit 3; -->

<!-- 5. Sekarang buatlah sebuah daftar semua anggota Congress yang setidaknya mendapatkan beberapa vote dalam urutan dari yang paling sedikit. Dan juga jangan sertakan field-field yang memiliki tipe date. -->
<!-- select congress_members.id,name,party,location,grade_1996,grade_current,years_in_congress,dw1_score,count(*) as number_of_votes from votes join congress_members on votes.politician_id = congress_members.id group by votes.politician_id order by count(*) desc limit 3; -->

<!-- Release 2  -->

<!-- 1. Siapa anggota Congress yang mendapatkan vote terbanyak? List nama mereka dan jumlah vote-nya. Siapa saja yang memilih politisi tersebut? List nama mereka, dan jenis kelamin mereka. -->
<!-- select first_name,last_name,gender,politician_id,name,count(*) as total_votes from votes join voters on votes.voter_id = voters.id join congress_members on votes.politician_id = congress_members.id group by votes.politician_id order by count(*); -->

<!-- 2. Berapa banyak vote yang diterima anggota Congress yang memiliki grade di bawah 9 (gunakan field `grade_current`)? Ambil nama, lokasi, grade_current dan jumlah vote. -->
<!-- select politician_id,name,location,grade_current,count(*) as total_votes from votes join congress_members on politician_id = congress_members.id where grade_current < 9 group by politician_id; -->

<!-- 3. Apa saja 10 negara bagian yang memiliki voters terbanyak? List semua orang yang melakukan vote di negara bagian yang paling populer. (Akan menjadi daftar yang panjang, kamu bisa gunakan hasil dari query pertama untuk menyederhanakan query berikut ini.) -->
<!-- with most_votes as (select location, count(*) as total_votes from congress_members join votes on congress_members.id = politician_id group by location order by count(*) desc limit 10)
select (first_name || ' ' || last_name) as voter,gender,politician_id,name,most_votes.location,total_votes from votes join congress_members on congress_members.id = politician_id join voters on voter_id = voters.id join most_votes on most_votes.location = congress_members.location order by total_votes desc; -->

<!-- 4. List orang-orang yang vote lebih dari dua kali. Harusnya mereka hanya bisa vote untuk posisi Senator dan satu lagi untuk wakil. Wow, kita dapat si tukang curang! Segera laporkan ke KPK!! -->
<!-- select first_name,last_name,gender,name,count(*) as total_votes from votes join voters on voter_id = voters.id join congress_members on congress_members.id = politician_id group by voter_id,politician_id having count(*) > 1 order by count(*) desc; -->
