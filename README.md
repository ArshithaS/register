# register
@app.route('/register', methods=['GET', 'POST'])
def register():
    if request.method == 'POST':
        name = request.form['name']
        username = request.form['username']
        password = request.form['password']
        with sqlite3.connect("database.db") as conn:
            try:
                conn.execute("INSERT INTO users (name, username, password, role) VALUES (?, ?, ?, ?)",
                             (name, username, password, "user"))
                conn.commit()
                flash("Registration successful! Please log in.", "success")
                return redirect(url_for('login'))
            except sqlite3.IntegrityError:
                flash("Username already exists.", "danger")
    return render_template('register.html')
