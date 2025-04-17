Penjelasan Terkait Tugas : 
✨ Fitur Tambahan: Filter Tampilan Tugas (Latihan 12)
✅ Deskripsi Fitur
Fitur ini memungkinkan pengguna untuk memfilter daftar To-Do berdasarkan status tugas:
Semua: Menampilkan semua tugas.
Selesai: Hanya menampilkan tugas yang sudah ditandai selesai.
Belum Selesai: Hanya menampilkan tugas yang belum selesai.

🛠️ Perubahan yang Dilakukan
1. HTML (Thymeleaf Template) – index.html
Ditambahkan elemen navigasi filter:

<div>
    <a href="/" th:href="@{/?filter=all}">Semua</a> |
    <a href="/" th:href="@{/?filter=completed}">Selesai</a> |
    <a href="/" th:href="@{/?filter=incomplete}">Belum Selesai</a>
</div>

2. Controller – ToDoController.java
Modifikasi method home() agar bisa menangani query filter dari URL:

@GetMapping
public String home(@RequestParam(value = "filter", required = false, defaultValue = "all") String filter, Model model) {
    List<ToDo> todos;

    switch (filter) {
        case "completed":
            todos = toDoService.getCompletedTodos();
            break;
        case "incomplete":
            todos = toDoService.getIncompleteTodos();
            break;
        default:
            todos = toDoService.getAllTodos();
            break;
    }

    model.addAttribute("todos", todos);
    model.addAttribute("newTodo", new ToDo());
    return "index";
}

3. Service – ToDoService.java
Menambahkan dua method baru:

public List<ToDo> getCompletedTodos() {
    return toDoRepository.findByCompleted(true);
}

public List<ToDo> getIncompleteTodos() {
    return toDoRepository.findByCompleted(false);
}

4. Repository – ToDoRepository.java
Menambahkan method query berdasarkan status:

List<ToDo> findByCompleted(boolean completed);

🧪 Hasil Saat Dijalankan
Setelah aplikasi dijalankan, halaman utama akan menampilkan 3 link di bagian atas:
[Semua] | [Selesai] | [Belum Selesai]

Klik Semua akan menampilkan seluruh tugas.
Klik Selesai hanya menampilkan tugas yang sudah ditandai ✔.
Klik Belum Selesai hanya menampilkan tugas yang belum ditandai.
