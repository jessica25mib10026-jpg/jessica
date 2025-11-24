from flask import Flask, request, render_template_string, redirect, url_for

app = Flask(__name__)

TEMPLATE = """
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Time Management Quiz</title>
  <style>
    body{font-family: system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial; background:#f7fafc; color:#0f172a;}
    .container{max-width:760px;margin:36px auto;padding:24px;background:#fff;border-radius:12px;box-shadow:0 6px 24px rgba(15,23,42,0.06);}
    h1{font-size:1.6rem;margin-bottom:6px}
    p.lead{color:#475569;margin-top:0;margin-bottom:18px}
    form > fieldset{border:none;margin:16px 0;padding:0}
    legend{font-weight:600;margin-bottom:8px}
    .option{display:block;margin:6px 0}
    button{background:#0ea5a4;color:white;border:none;padding:10px 16px;border-radius:8px;font-weight:600;cursor:pointer}
    .result{margin-top:18px;padding:14px;border-radius:10px;background:#ecfeff;border:1px solid #a7f3d0}
    .tip{background:#f1f5f9;padding:12px;border-radius:8px;margin-top:10px}
    small.muted{color:#64748b}
  </style>
</head>
<body>
  <div class="container">
    <h1>Time Management Quiz</h1>
    <p class="lead">Answer the questions honestly to get a small personalised tip at the end.</p>

    {% if result is not defined %}
    <form method="post" action="{{ url_for('quiz') }}">
      <fieldset>
        <legend>1) What is your wake up routine time?</legend>
        <label class="option"><input type="radio" name="q1" value="a" required> a) yes, always</label>
        <label class="option"><input type="radio" name="q1" value="b"> b) not properly</label>
      </fieldset>

      <fieldset>
        <legend>2) Do you follow your breakfast timing all day properly?</legend>
        <label class="option"><input type="radio" name="q2" value="a" required> a) yes I do</label>
        <label class="option"><input type="radio" name="q2" value="b"> b) not all day</label>
      </fieldset>

      <fieldset>
        <legend>3) Do you manage your exam timing?</legend>
        <label class="option"><input type="radio" name="q3" value="a" required> a) yes I do</label>
        <label class="option"><input type="radio" name="q3" value="b"> b) not all the time</label>
      </fieldset>

      <fieldset>
        <legend>4) What is your sleep cycle?</legend>
        <label class="option"><input type="radio" name="q4" value="a" required> a) 9-10 pm</label>
        <label class="option"><input type="radio" name="q4" value="b"> b) 10-11 pm</label>
        <label class="option"><input type="radio" name="q4" value="c"> c) 11pm-12am</label>
      </fieldset>

      <button type="submit">Submit &amp; See Results</button>
    </form>
    <p style="margin-top:12px"><small class="muted">This simple demo doesn't store answers on the server.</small></p>

    {% else %}

    <div class="result">
      <h2>Your result: {{ score }} / 4</h2>
      <p>{{ verdict }}</p>

      <div class="tip">
        <strong>Personalised tips</strong>
        <ul>
          {% for t in tips %}
          <li>{{ t }}</li>
          {% endfor %}
        </ul>
      </div>

      <p style="margin-top:12px"><a href="{{ url_for('quiz') }}">Retake quiz</a></p>
    </div>

    {% endif %}

  </div>
</body>
</html>
"""


def evaluate(answers):
    # Each 'good' answer gives 1 point. Map answers to points.
    score = 0
    tips = []

    # Q1: 'a' -> good
    if answers.get('q1') == 'a':
        score += 1
    else:
        tips.append('Try setting a consistent wake-up time and use an alarm you can rely on.')

    # Q2: 'a' -> good
    if answers.get('q2') == 'a':
        score += 1
    else:
        tips.append('Regular breakfast helps stabilise energy — plan quick healthy options if mornings are rushed.')

    # Q3: 'a' -> good
    if answers.get('q3') == 'a':
        score += 1
    else:
        tips.append('Practice timed mock tests to improve time management under exam conditions.')

    # Q4: earlier sleep is better. 'a' best, 'b' okay, 'c' later
    q4 = answers.get('q4')
    if q4 == 'a':
        score += 1
    elif q4 == 'b':
        score += 0  # neutral
        tips.append('If possible, aim for a slightly earlier bedtime to ensure quality rest.')
    else:
        tips.append('Try winding down earlier: reduce screens 30–60 minutes before sleep and keep a regular sleep schedule.')

    # Generate verdict
    if score == 4:
        verdict = 'Excellent! You have a solid time-management routine.'
    elif 2 <= score < 4:
        verdict = 'Good — you have some habits in place. A little refinement will help.'
    else:
        verdict = 'There's room for improvement. Start with one small habit change at a time.'

    # General tips if none added
    if not tips:
        tips = ['Keep monitoring your routine and be consistent — small habits compound.']

    return score, verdict, tips


@app.route('/', methods=['GET', 'POST'])
def quiz():
    if request.method == 'POST':
        answers = request.form.to_dict()
        score, verdict, tips = evaluate(answers)
        return render_template_string(TEMPLATE, result=True, score=score, verdict=verdict, tips=tips)
    return render_template_string(TEMPLATE)


if __name__ == '__main__':
    # For development only. In production use a WSGI server.
    app.run(debug=True)
