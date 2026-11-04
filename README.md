
<!DOCTYPE html>
<html lang="pl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Mój Dziennik</title>

<style>
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}

body {
    font-family: Arial, sans-serif;
    background: #f1f4f9;
    color: #263449;
}

header {
    background: #172d50;
    color: white;
    padding: 22px 35px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 15px;
}

header h1 { font-size: 23px; }

header span {
    color: #b9c9df;
    font-size: 13px;
}

.logout {
    background: #ffffff20;
    border: 1px solid #ffffff55;
    color: white;
    border-radius: 7px;
    padding: 10px 14px;
    cursor: pointer;
}

main {
    max-width: 1050px;
    margin: 35px auto;
    padding: 0 20px;
}

h2 {
    margin-bottom: 22px;
    font-size: 24px;
}

.card {
    background: white;
    border-radius: 12px;
    padding: 22px;
    margin-bottom: 20px;
    box-shadow: 0 2px 8px #172d500d;
}

.subject-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.muted {
    color: #8491a3;
    font-size: 12px;
    margin-top: 5px;
}

.average {
    margin-top: 9px;
    color: #2459a6;
    font-size: 14px;
    font-weight: bold;
}

.add-grade {
    width: 40px;
    height: 40px;
    border: none;
    border-radius: 8px;
    background: #2459a6;
    color: white;
    font-size: 25px;
    cursor: pointer;
}

.grade-bar {
    display: flex;
    flex-wrap: wrap;
    gap: 12px;
    margin-top: 20px;
}

.grade-entry {
    position: relative;
}

.grade-pill {
    display: inline-flex;
    justify-content: center;
    align-items: center;
    min-width: 42px;
    height: 42px;
    padding: 0 12px;
    background: #e7effb;
    color: #2459a6;
    border-radius: 8px;
    font-size: 16px;
    font-weight: bold;
    cursor: pointer;
}

.grade-tooltip {
    display: none;
    position: absolute;
    bottom: 52px;
    left: 0;
    width: 250px;
    background: #172d50;
    color: white;
    padding: 13px;
    border-radius: 9px;
    z-index: 20;
    font-size: 12px;
    line-height: 1.8;
    box-shadow: 0 5px 18px #0002;
}

.grade-entry:hover .grade-tooltip {
    display: block;
}

.grade-delete {
    position: absolute;
    top: -7px;
    right: -7px;
    width: 20px;
    height: 20px;
    border: none;
    border-radius: 50%;
    background: #dc4444;
    color: white;
    font-size: 15px;
    line-height: 20px;
    cursor: pointer;
    display: none;
    z-index: 30;
}

.grade-entry:hover .grade-delete {
    display: block;
}

.modal-overlay {
    display: none;
    position: fixed;
    inset: 0;
    background: #0008;
    z-index: 100;
    align-items: center;
    justify-content: center;
    padding: 15px;
}

.modal-overlay.show { display: flex; }

.modal-box {
    background: white;
    width: 100%;
    max-width: 470px;
    max-height: 90vh;
    overflow-y: auto;
    border-radius: 13px;
    padding: 24px;
}

.modal-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 22px;
}

.modal-header h2 { margin: 0; }

.modal-close {
    border: none;
    background: none;
    font-size: 28px;
    cursor: pointer;
    color: #718096;
}

label {
    font-size: 13px;
    font-weight: bold;
}

input, select, textarea {
    width: 100%;
    padding: 12px;
    border: 1px solid #dce2eb;
    border-radius: 7px;
    margin: 7px 0 14px;
    font-size: 14px;
    font-family: inherit;
}

textarea {
    min-height: 90px;
    resize: vertical;
}

.primary {
    background: #2459a6;
    color: white;
    border: none;
    border-radius: 7px;
    padding: 12px 18px;
    cursor: pointer;
    font-size: 14px;
}

.primary:hover { background: #194782; }

.add-submit {
    width: 100%;
    margin-top: 10px;
}

.empty {
    color: #8995a5;
    padding: 18px 0;
    font-size: 13px;
}

/* LOGOWANIE */

#loginScreen {
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 20px;
    background: #f1f4f9;
}

.login-box {
    width: 100%;
    max-width: 390px;
    background: white;
    border-radius: 15px;
    padding: 32px;
    box-shadow: 0 8px 35px #172d5018;
}

.login-box h1 {
    color: #172d50;
    font-size: 25px;
    margin-bottom: 8px;
}

.login-box p {
    color: #8491a3;
    font-size: 14px;
    margin-bottom: 25px;
}

.login-box button {
    width: 100%;
}

.login-error {
    display: none;
    color: #d13b3b;
    font-size: 13px;
    margin-bottom: 12px;
}

#app {
    display: none;
}

@media(max-width:600px) {
    header {
        padding: 18px;
        flex-wrap: wrap;
    }

    main { margin-top: 22px; }
    .login-box { padding: 24px; }
}
</style>
</head>

<body>

<!-- LOGOWANIE -->

<div id="loginScreen">
    <div class="login-box">
        <h1>📘 Mój Dziennik</h1>
        <p>Zaloguj się do swojego konta</p>

        <form id="loginForm">
            <label>Login</label>
            <input id="login" autocomplete="username" placeholder="Wpisz login" required>

            <label>Hasło</label>
            <input id="password" type="password" autocomplete="current-password" placeholder="Wpisz hasło" required>

            <div id="loginError" class="login-error">
                Nieprawidłowy login lub hasło!
            </div>

            <button class="primary" type="submit">Zaloguj się</button>
        </form>
    </div>
</div>

<!-- APLIKACJA -->

<div id="app">

<header>
    <div>
        <h1>📘 Mój Dziennik</h1>
        <span id="accountInfo"></span>
    </div>

    <button class="logout" onclick="logout()">Wyloguj się</button>
</header>

<main>

<section id="grades">

    <h2 id="gradesTitle">Oceny</h2>

    <div class="card">
        <div class="subject-header">
            <div>
                <h3>Matematyka</h3>
                <div class="muted">Oceny z przedmiotu</div>
                <div class="average" id="average-matematyka">Średnia: 0,00</div>
            </div>

            <button class="add-grade teacher-only"
                    onclick="openGradeModal('Matematyka')">+</button>
        </div>

        <div id="grades-matematyka" class="grade-bar"></div>
    </div>

    <div class="card">
        <div class="subject-header">
            <div>
                <h3>Język polski</h3>
                <div class="muted">Oceny z przedmiotu</div>
                <div class="average" id="average-polski">Średnia: 0,00</div>
            </div>

            <button class="add-grade teacher-only"
                    onclick="openGradeModal('Język polski')">+</button>
        </div>

        <div id="grades-polski" class="grade-bar"></div>
    </div>

</section>

</main>
</div>

<!-- OKNO DODAWANIA OCENY -->

<div id="gradeModal" class="modal-overlay">

    <div class="modal-box">

        <div class="modal-header">
            <h2>Dodaj ocenę</h2>
            <button onclick="closeGradeModal()" class="modal-close">×</button>
        </div>

        <form id="gradeForm">

            <label>Uczeń</label>
            <select id="studentName" required>
                <option value="Bogusia Siuda">Bogusia Siuda</option>
            </select>

            <label>Rodzaj oceny</label>
            <select id="gradeType">
                <option value="Zwykła ocena">Zwykła ocena</option>
                <option value="Ocena na koniec roku">Ocena na koniec roku</option>
            </select>

            <label>Przedmiot</label>
            <select id="gradeSubject">
                <option value="Matematyka">Matematyka</option>
                <option value="Język polski">Język polski</option>
            </select>

            <label>Ocena</label>
            <select id="newGrade">
                <option>1</option>
                <option>1+</option>
                <option>2-</option>
                <option>2</option>
                <option>2+</option>
                <option>3-</option>
                <option>3</option>
                <option>3+</option>
                <option>4-</option>
                <option>4</option>
                <option>4+</option>
                <option>5-</option>
                <option>5</option>
                <option>5+</option>
                <option>6-</option>
                <option>6</option>
            </select>

            <label>Umiejętność</label>
            <select id="gradeSkill">
                <option>Kartkówka</option>
                <option>Sprawdzian</option>
                <option>Odpowiedź ustna</option>
                <option>Inne</option>
            </select>

            <label>Komentarz</label>
            <textarea id="gradeComment" placeholder="Z czego jest ocena?"></textarea>

            <label>Waga oceny</label>
            <select id="gradeWeight">
                <option value="1">1</option>
                <option value="2">2</option>
                <option value="3">3</option>
            </select>

            <button type="submit" class="primary add-submit">Dodaj ocenę</button>

        </form>
    </div>
</div>

<script>

// KONTA DEMONSTRACYJNE
// W prawdziwej aplikacji logowanie musi być sprawdzane na serwerze.

const accounts = {
    pawelwal: {
        password: "gwpnwLOZ1!",
        role: "teacher",
        name: "Paweł Waloszczyk"
    },
    bs: {
        password: "1234",
        role: "student",
        name: "Bogusia Siuda"
    }
};

let currentUser = null;

// DANE

let grades = JSON.parse(localStorage.getItem("grades")) || [];

function save() {
    localStorage.setItem("grades", JSON.stringify(grades));
}

// LOGOWANIE

document.getElementById("loginForm").addEventListener("submit", function(e) {
    e.preventDefault();

    const login = document.getElementById("login").value.trim();
    const password = document.getElementById("password").value;

    const account = accounts[login];

    if (!account || account.password !== password) {
        document.getElementById("loginError").style.display = "block";
        return;
    }

    currentUser = {
        login: login,
        role: account.role,
        name: account.name
    };

    document.getElementById("loginScreen").style.display = "none";
    document.getElementById("app").style.display = "block";

    document.getElementById("accountInfo").textContent =
        (currentUser.role === "teacher" ? "Konto nauczyciela – " : "Konto ucznia – ")
        + currentUser.name;

    document.getElementById("gradesTitle").textContent =
        currentUser.role === "teacher" ? "Oceny uczniów" : "Moje oceny";

    document.querySelectorAll(".teacher-only").forEach(element => {
        element.style.display =
            currentUser.role === "teacher" ? "block" : "none";
    });

    renderGrades();
});

function logout() {
    currentUser = null;

    document.getElementById("app").style.display = "none";
    document.getElementById("loginScreen").style.display = "flex";

    document.getElementById("login").value = "";
    document.getElementById("password").value = "";
    document.getElementById("loginError").style.display = "none";

    closeGradeModal();
}

// OKNO OCEN

function openGradeModal(subject) {
    if (!currentUser || currentUser.role !== "teacher") return;

    document.getElementById("gradeSubject").value = subject;
    document.getElementById("gradeModal").classList.add("show");
}

function closeGradeModal() {
    document.getElementById("gradeModal").classList.remove("show");
}

document.getElementById("gradeModal").addEventListener("click", function(e) {
    if (e.target === this) closeGradeModal();
});

// DODAWANIE OCEN

document.getElementById("gradeForm").addEventListener("submit", function(e) {
    e.preventDefault();

    if (!currentUser || currentUser.role !== "teacher") return;

    grades.push({
        student: document.getElementById("studentName").value,
        subject: document.getElementById("gradeSubject").value,
        grade: document.getElementById("newGrade").value,
        type: document.getElementById("gradeType").value,
        skill: document.getElementById("gradeSkill").value,
        comment: document.getElementById("gradeComment").value.trim(),
        weight: document.getElementById("gradeWeight").value,
        addedBy: currentUser.name,
        date: new Date().toLocaleString("pl-PL")
    });

    save();
    renderGrades();

    this.reset();
    closeGradeModal();
});

// WARTOŚĆ OCENY

function gradeNumber(grade) {
    let number = parseInt(grade);

    if (grade.includes("+")) number += 0.5;
    if (grade.includes("-")) number -= 0.25;

    return number;
}

// USUWANIE OCENY

function deleteGrade(index) {
    if (!currentUser || currentUser.role !== "teacher") return;

    if (!confirm("Czy na pewno chcesz usunąć tę ocenę?")) return;

    grades.splice(index, 1);
    save();
    renderGrades();
}

// WYŚWIETLANIE OCEN

function renderGrades() {

    if (!currentUser) return;

    const subjects = [
        {
            name: "Matematyka",
            id: "grades-matematyka",
            average: "average-matematyka"
        },
        {
            name: "Język polski",
            id: "grades-polski",
            average: "average-polski"
        }
    ];

    subjects.forEach(subject => {

        const list = document.getElementById(subject.id);
        list.innerHTML = "";

        const subjectGrades = grades
            .map((g, index) => ({...g, originalIndex: index}))
            .filter(g => {
                if (g.subject !== subject.name) return false;

                if (currentUser.role === "student") {
                    return g.student === currentUser.name;
                }

                return true;
            });

        const countedGrades = subjectGrades.filter(g =>
            g.type !== "Ocena na koniec roku"
        );

        let average = 0;

        if (countedGrades.length) {
            average = countedGrades.reduce((sum, g) =>
                sum + gradeNumber(g.grade), 0
            ) / countedGrades.length;
        }

        document.getElementById(subject.average).textContent =
            "Średnia: " + average.toFixed(2).replace(".", ",");

        if (!subjectGrades.length) {
            list.textContent = "Brak ocen";
        }

        subjectGrades.forEach(g => {

            const entry = document.createElement("div");
            entry.className = "grade-entry";

            const pill = document.createElement("span");
            pill.className = "grade-pill";
            pill.textContent = g.grade;

            const tooltip = document.createElement("div");
            tooltip.className = "grade-tooltip";

            const details = [
                ["Uczeń", g.student],
                ["Dodał", g.addedBy],
                ["Ocena", g.grade],
                ["Rodzaj", g.type],
                ["Z czego", g.skill],
                ["Komentarz", g.comment || "Brak"],
                ["Waga", g.weight],
                ["Data dodania", g.date]
            ];

            details.forEach(([label, value]) => {
                const line = document.createElement("div");
                const strong = document.createElement("strong");

                strong.textContent = label + ": ";

                line.appendChild(strong);
                line.appendChild(document.createTextNode(value ?? ""));

                tooltip.appendChild(line);
            });

            entry.appendChild(pill);
            entry.appendChild(tooltip);

            if (currentUser.role === "teacher") {
                const deleteBtn = document.createElement("button");

                deleteBtn.className = "grade-delete";
                deleteBtn.textContent = "×";
                deleteBtn.title = "Usuń ocenę";

                deleteBtn.onclick = function() {
                    deleteGrade(g.originalIndex);
                };

                entry.appendChild(deleteBtn);
            }

            list.appendChild(entry);
        });
    });
}

</script>

</body>
</html>
