<!-- Release 1  -->

<!-- 1. Hitung jumlah vote untuk Sen. Olympia Snowe yang memiliki id 524. -->
SELECT COUNT(*) as sum_of_votes FROM votes WHERE politician_id = 524;

<!-- 2. Sekarang lakukan JOIN tanpa menggunakan id `524`. Query kedua tabel votes dan congress_members. -->

SELECT SELECT * FROM votes JOIN congress_members ON votes.politician_id = congress_members.id WHERE congress_members.name = 'Sen. Olympia Snowe';

<!-- 3. Sekarang gimana dengan representative Erik Paulsen? Berapa banyak vote yang dia dapatkan? -->

SELECT COUNT(politician_id) FROM votes WHERE politician_id = (SELECT id FROM congress_members WHERE name='Rep. Erik Paulsen')

<!-- 4. Buatlah daftar peserta Congress yang mendapatkan vote terbanyak. Jangan sertakan field `created_at` dan `updated_at`. -->

SELECT name, party, location, grade_1996, grade_current,years_in_congress, dw1_score, COUNT(politician_id) AS numbers_in_vote FROM congress_members, votes WHERE congress_members.id = votes.politician_id GROUP BY congress_members.id ORDER BY numbers_in_vote DESC LIMIT 3;

<!-- 5. Sekarang buatlah sebuah daftar semua anggota Congress yang setidaknya mendapatkan beberapa vote dalam urutan dari yang paling sedikit. Dan juga jangan sertakan field-field yang memiliki tipe date. -->

SELECT name, party, location, grade_1996, grade_current,years_in_congress, dw1_score, COUNT(politician_id) AS numbers_in_vote FROM congress_members, votes WHERE congress_members.id = votes.politician_id GROUP BY congress_members.id ORDER BY numbers_in_vote ASC LIMIT 3;
