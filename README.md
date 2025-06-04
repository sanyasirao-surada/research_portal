import os
from flask import Flask, render_template, redirect, url_for, flash, request, send_from_directory
from flask_sqlalchemy import SQLAlchemy
from flask_wtf import FlaskForm
from wtforms import StringField, TextAreaField, IntegerField, FileField, SubmitField
from wtforms.validators import DataRequired, URL, Optional, NumberRange
from werkzeug.utils import secure_filename

from config import Config

# ─── Flask App Setup ────────────────────────────────────────────────────────────
app = Flask(__name__)
app.config.from_object(Config)

db = SQLAlchemy(app)

# Ensure upload folder exists
os.makedirs(app.config["UPLOAD_FOLDER"], exist_ok=True)

# ─── Database Model ─────────────────────────────────────────────────────────────
class Publication(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(300), nullable=False)
    authors = db.Column(db.String(500), nullable=False)
    journal = db.Column(db.String(200), nullable=False)
    year = db.Column(db.Integer, nullable=False)
    doi = db.Column(db.String(200), nullable=True)
    abstract = db.Column(db.Text, nullable=True)
    pdf_filename = db.Column(db.String(300), nullable=True)  # store filename in uploads/

    def __repr__(self):
        return f"<Publication {self.title}>"

# ─── WTForms Form ────────────────────────────────────────────────────────────────
def allowed_file(filename):
    return "." in filename and filename.rsplit(".", 1)[1].lower() in app.config["ALLOWED_EXTENSIONS"]

class PublicationForm(FlaskForm):
    title = StringField("Title", validators=[DataRequired()])
    authors = StringField("Authors", validators=[DataRequired()])
    journal = StringField("Journal/Conference Name", validators=[DataRequired()])
    year = IntegerField("Year", validators=[DataRequired(), NumberRange(min=1900, max=2100)])
    doi = StringField("DOI (optional)", validators=[Optional()])
    abstract = TextAreaField("Abstract (optional)", validators=[Optional()])
    pdf_file = FileField("Upload PDF (optional, .pdf only)", validators=[Optional()])
    submit = SubmitField("Submit Publication")

# ─── Routes ─────────────────────────────────────────────────────────────────────
@app.route("/")
def index():
    """Homepage: lists all publications."""
    publications = Publication.query.order_by(Publication.year.desc()).all()
    return render_template("index.html", publications=publications)

@app.route("/upload", methods=["GET", "POST"])
def upload_publication():
    """Form page to upload a new publication."""
    form = PublicationForm()
    if form.validate_on_submit():
        # Create a new Publication object
        pub = Publication(
            title=form.title.data.strip(),
            authors=form.authors.data.strip(),
            journal=form.journal.data.strip(),
            year=form.year.data,
            doi=form.doi.data.strip() if form.doi.data else None,
            abstract=form.abstract.data.strip() if form.abstract.data else None,
        )

        # Handle PDF upload (if any)
        file = form.pdf_file.data
        if file:
            filename = secure_filename(file.filename)
            if allowed_file(filename):
                # Prefix filename with publication ID or timestamp to avoid collisions
                unique_name = f"{pub.title[:30].replace(' ', '_')}_{pub.year}_{filename}"
                save_path = os.path.join(app.config["UPLOAD_FOLDER"], unique_name)
                file.save(save_path)
                pub.pdf_filename = unique_name
            else:
                flash("Only PDF files are allowed for upload.", "danger")
                return redirect(request.url)

        # Add to DB
        db.session.add(pub)
        db.session.commit()
        flash("Publication added successfully!", "success")
        return redirect(url_for("index"))

    return render_template("upload.html", form=form)

@app.route("/uploads/<filename>")
def download_file(filename):
    """Serve uploaded PDF files."""
    return send_from_directory(app.config["UPLOAD_FOLDER"], filename, as_attachment=False)

# ─── Initialize DB ──────────────────────────────────────────────────────────────
@app.before_first_request
def create_tables():
    db.create_all()

if __name__ == "__main__":
    app.run(debug=True)
