from flask import Flask, render_template, request, redirect, url_for
import sqlite3

app = Flask(__name__)
app.config['SECRET_KEY'] = 'secret_key'
app.config['DATABASE'] = 'database.db'

def get_db():
    db = sqlite3.connect(app.config['DATABASE'])
    db.row_factory = sqlite3.Row
    return db

@app.route('/')
def home():
    db = get_db()
    posts = db.execute('SELECT * FROM posts ORDER BY date_posted DESC').fetchall()
    return render_template('home.html', posts=posts)

@app.route('/post/<int:id>')
def post(id):
    db = get_db()
    post = db.execute('SELECT * FROM posts WHERE id = ?', (id,)).fetchone()
    return render_template('post.html', post=post)

@app.route('/create_post', methods=['GET', 'POST'])
def create_post():
    if request.method == 'POST':
        title = request.form['title']
        content = request.form['content']
        db = get_db()
        db.execute('INSERT INTO posts (title, content) VALUES (?, ?)', (title, content))
        db.commit()
        return redirect(url_for('home'))
    else:
        return render_template('create_post.html')

if __name__ == '__main__':
    app.run(debug=True)
