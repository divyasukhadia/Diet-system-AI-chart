# Diet-system-AI-chart CODE
import streamlit as st

# ---------------- PAGE CONFIG ----------------
st.set_page_config(
    page_title="AI Diet Recommendation System",
    layout="wide"
)

st.title("🍅 AI-Based Weekly Diet & Exercise Recommendation System")

# ---------------- BUBBLE CARD CSS ----------------
st.markdown("""
<style>
.bubble-card {
    background: #0f172a;
    border-radius: 18px;
    padding: 18px;
    margin-bottom: 18px;
    box-shadow: 0 10px 30px rgba(0,0,0,0.35);
    color: white;
    animation: floatUp 0.6s ease;
}
@keyframes floatUp {
    from {opacity: 0; transform: translateY(20px);}
    to {opacity: 1; transform: translateY(0);}
}
</style>
""", unsafe_allow_html=True)

# ---------------- INPUTS ----------------
st.subheader("👤 Personal Details")

c1, c2, c3 = st.columns(3)

with c1:
    gender = st.radio("Gender", ["male", "female"])
    on_period = False
    if gender == "female":
        on_period = st.checkbox("🔴 I am on my period")

    food_type = st.radio("Food Preference", ["veg", "non-veg", "vegan"])

with c2:
    lifestyle = st.selectbox("Lifestyle", ["student", "regular", "athlete"])
    goal = st.selectbox("Goal", ["gain", "maintain", "loss"])

with c3:
    age = st.number_input("Age", 10, 80, 25)
    height = st.number_input("Height (cm)", 0.0)
    weight = st.number_input("Weight (kg)", 0.0)

generate = st.button("📅 Generate Weekly Plan")

if not generate:
    st.stop()

# ---------------- VALIDATION ----------------
if height <= 0 or weight <= 0:
    st.error("❌ Please enter valid height and weight")
    st.stop()

# ---------------- BMI ----------------
bmi = weight / ((height / 100) ** 2)

if bmi < 18.5:
    bmi_cat = "underweight"
elif bmi < 25:
    bmi_cat = "normal"
elif bmi < 30:
    bmi_cat = "overweight"
else:
    bmi_cat = "obese"

st.subheader("📊 Health Summary")
st.success(f"BMI: {bmi:.2f} | Category: {bmi_cat.capitalize()}")

# ---------------- FOOD DATABASE ----------------
# Structure:
# food_type -> bmi -> goal -> day -> (breakfast, lunch, dinner)

FOOD_DB = {

    # ================= VEG =================
    "veg": {
        "underweight": {
            "gain": {
                "Monday": ("Milk + banana + nuts", "Rice + dal + ghee", "Paneer curry"),
                "Tuesday": ("Oats + fruits", "Veg pulao + curd", "Paneer bhurji"),
                "Wednesday": ("Poha + peanuts", "Rajma + rice", "Veg curry"),
                "Thursday": ("Smoothie + nuts", "Khichdi + ghee", "Sabzi + roti"),
                "Friday": ("Upma", "Rice + dal", "Paneer tikka"),
                "Saturday": ("Dosa + chutney", "Veg biryani", "Curd rice"),
                "Sunday": ("Paratha + curd", "Home thali", "Light sabzi"),
            }
        },
        "normal": {
            "maintain": {
                "Monday": ("Oats + fruits", "Rice + dal", "Chapati + sabzi"), 
                "Tuesday": ("Idli + sambar", "Veg rice", "Curd + roti"),
                "Wednesday": ("Poha", "Rajma", "Veg curry"),
                "Thursday": ("Smoothie bowl", "Khichdi", "Veg stir fry"),
                "Friday": ("Upma", "Dal rice", "Sabzi"),
                "Saturday": ("Dosa", "Veg pulao", "Light dinner"),
                "Sunday": ("Paratha", "Home thali", "Soup"),
            }
        },
        "overweight": {
            "loss": {
                "Monday": ("Fruits", "Salad + dal", "Soup"),
                "Tuesday": ("Oats", "Veg soup", "Grilled veg"),
                "Wednesday": ("Smoothie", "Salad", "Soup"),
                "Thursday": ("Green tea + fruit", "Light khichdi", "Steamed veg"),
                "Friday": ("Fruit bowl", "Salad", "Soup"),
                "Saturday": ("Oats porridge", "Veg curry", "Light dinner"),
                "Sunday": ("Idli", "Simple veg meal", "Soup"),
            }
        },
        "obese": {
            "loss": {
                "Monday": ("Green tea + fruit", "Salad", "Veg soup"),
                "Tuesday": ("Fruit bowl", "Boiled veg", "Soup"),
                "Wednesday": ("Oats", "Salad", "Soup"),
                "Thursday": ("Smoothie", "Light khichdi", "Soup"),
                "Friday": ("Fruits", "Salad", "Veg soup"),
                "Saturday": ("Idli (1)", "Light veg meal", "Soup"),
                "Sunday": ("Poha (small)", "Simple veg meal", "Soup"),
            }
        }
    },

    # ================= NON-VEG =================
    "non-veg": {
        "underweight": {
            "gain": {
                "Monday": ("Eggs + banana", "Chicken curry + rice", "Chicken soup"),
                "Tuesday": ("Omelette + toast", "Fish curry + rice", "Boiled eggs"),
                "Wednesday": ("Egg bhurji", "Chicken biryani", "Grilled chicken"),
                "Thursday": ("Smoothie + eggs", "Egg curry + rice", "Chicken stir fry"),
                "Friday": ("Scrambled eggs", "Chicken pulao", "Fish fry"),
                "Saturday": ("Egg dosa", "Mutton curry", "Soup"),
                "Sunday": ("Egg sandwich", "Home chicken meal", "Light dinner"),
            }
        },
        "normal": {
            "maintain": {
                "Monday": ("Boiled eggs", "Chicken curry", "Grilled chicken"),
                "Tuesday": ("Omelette", "Fish curry", "Egg salad"),
                "Wednesday": ("Egg toast", "Chicken rice", "Soup"),
                "Thursday": ("Smoothie + eggs", "Fish + rice", "Grilled chicken"),
                "Friday": ("Egg bhurji", "Chicken dal", "Soup"),
                "Saturday": ("Omelette", "Fish pulao", "Light dinner"),
                "Sunday": ("Boiled eggs", "Home meal", "Soup"),
            }
        },
        "overweight": {
            "loss": {
                "Monday": ("Boiled eggs", "Grilled chicken salad", "Soup"),
                "Tuesday": ("Egg whites", "Fish curry", "Steamed chicken"),
                "Wednesday": ("Omelette", "Chicken salad", "Soup"),
                "Thursday": ("Egg whites", "Grilled fish", "Soup"),
                "Friday": ("Boiled eggs", "Chicken soup", "Soup"),
                "Saturday": ("Egg whites", "Fish curry", "Light dinner"),
                "Sunday": ("Omelette", "Chicken salad", "Soup"),
            }
        },
        "obese": {
            "loss": {
                "Monday": ("Egg whites", "Grilled chicken", "Soup"),
                "Tuesday": ("Boiled eggs", "Fish salad", "Soup"),
                "Wednesday": ("Egg whites", "Chicken soup", "Soup"),
                "Thursday": ("Omelette", "Grilled fish", "Soup"),
                "Friday": ("Egg whites", "Chicken salad", "Soup"),
                "Saturday": ("Boiled eggs", "Fish curry", "Soup"),
                "Sunday": ("Egg whites", "Chicken soup", "Soup"),
            }
        }
    },

    # ================= VEGAN =================
    "vegan": {
        "underweight": {
            "gain": {
                "Monday": ("Dates + nuts", "Lentil curry + rice", "Veg stew"),
                "Tuesday": ("Smoothie bowl", "Chickpea salad", "Tofu stir fry"),
                "Wednesday": ("Oats + seeds", "Rajma + rice", "Veg curry"),
                "Thursday": ("Peanut butter toast", "Bean curry", "Tofu"),
                "Friday": ("Fruit smoothie", "Lentils + rice", "Veg soup"),
                "Saturday": ("Upma", "Veg pulao", "Khichdi"),
                "Sunday": ("Dosa", "Home vegan thali", "Soup"),
            }
        },
        "normal": {
            "maintain": {
                "Monday": ("Oats + fruits", "Veg curry", "Soup"),
                "Tuesday": ("Smoothie", "Chickpea salad", "Tofu"),
                "Wednesday": ("Poha", "Rajma", "Veg curry"),
                "Thursday": ("Fruit bowl", "Khichdi", "Soup"),
                "Friday": ("Upma", "Dal rice", "Veg stir fry"),
                "Saturday": ("Dosa", "Veg pulao", "Light dinner"),
                "Sunday": ("Paratha", "Home vegan meal", "Soup"),
            }
        },
        "overweight": {
            "loss": {
                "Monday": ("Fruit bowl", "Salad", "Soup"),
                "Tuesday": ("Smoothie", "Veg soup", "Steamed veg"),
                "Wednesday": ("Oats", "Salad", "Soup"),
                "Thursday": ("Green tea + fruit", "Khichdi", "Soup"),
                "Friday": ("Fruits", "Veg curry", "Soup"),
                "Saturday": ("Idli", "Light meal", "Soup"),
                "Sunday": ("Poha", "Simple meal", "Soup"),
            }
        },
        "obese": {
            "loss": {
                "Monday": ("Fruit bowl", "Salad", "Soup"),
                "Tuesday": ("Smoothie", "Veg soup", "Soup"),
                "Wednesday": ("Oats", "Salad", "Soup"),
                "Thursday": ("Fruit + tea", "Khichdi", "Soup"),
                "Friday": ("Fruits", "Veg curry", "Soup"),
                "Saturday": ("Idli (1)", "Light meal", "Soup"),
                "Sunday": ("Poha (small)", "Simple meal", "Soup"),
            }
        }
    }

}

# ---------------- PERIOD OVERRIDE ----------------
PERIOD_FOOD = {
    "Monday": ("Dates + banana + warm milk", "Spinach dal + rice", "Veg khichdi"),
    "Tuesday": ("Oats + jaggery", "Beetroot curry", "Veg soup"),
    "Wednesday": ("Smoothie + nuts", "Rajma + rice", "Paneer + veg"),
    "Thursday": ("Poha + peanuts", "Palak dal", "Light sabzi"),
    "Friday": ("Dates + apple", "Chole + rice", "Veg soup"),
    "Saturday": ("Upma + seeds", "Veg pulao", "Light khichdi"),
    "Sunday": ("Dosa + chutney", "Home veg thali", "Soup"),
}

# ---------------- EXERCISE ----------------
EXERCISE = {
    "student": "Light workout (20 min)",
    "regular": "Brisk walking (30 min)",
    "athlete": "Strength training (45 min)",
}


def meal_meta(day):
    # Default timings & quantities
    meta = {
        "Breakfast": ("8:00 AM", "1 bowl / 2 items"),
        "Lunch": ("1:00 PM", "1 plate"),
        "Dinner": ("8:00 PM", "1 light bowl"),
    }

    # Adjust quantities by BMI & goal
    if bmi_cat == "underweight" and goal == "gain":
        meta["Breakfast"] = ("8:00 AM", "1 large bowl")
        meta["Lunch"] = ("1:00 PM", "1.5 plates")
        meta["Dinner"] = ("8:30 PM", "1 full bowl")

    if bmi_cat in ["overweight", "obese"] and goal == "loss":
        meta["Breakfast"] = ("8:30 AM", "1 small bowl")
        meta["Lunch"] = ("1:30 PM", "1 light plate")
        meta["Dinner"] = ("7:30 PM", "Soup / very light")

    # Period adjustment
    if gender == "female" and on_period:
        meta["Breakfast"] = ("8:00 AM", "Iron-rich serving")
        meta["Lunch"] = ("1:00 PM", "Balanced plate")
        meta["Dinner"] = ("8:00 PM", "Light & warm")

    return meta


def get_recommended_food(day):
    if gender == "female" and on_period:
        return PERIOD_FOOD.get(
            day,
            ("Iron-rich breakfast", "Balanced lunch", "Light dinner")
        )

    # Exact match
    if (
        food_type in FOOD_DB and
        bmi_cat in FOOD_DB[food_type] and
        goal in FOOD_DB[food_type][bmi_cat] and
        day in FOOD_DB[food_type][bmi_cat][goal]
    ):
        return FOOD_DB[food_type][bmi_cat][goal][day]

    # Fallback 1: same BMI, maintain
    if (
        food_type in FOOD_DB and
        bmi_cat in FOOD_DB[food_type] and
        "maintain" in FOOD_DB[food_type][bmi_cat]
    ):
        return FOOD_DB[food_type][bmi_cat]["maintain"].get(
            day, ("Balanced breakfast", "Balanced lunch", "Balanced dinner")
        )

    # Fallback 2: normal BMI
    if (
        food_type in FOOD_DB and
        "normal" in FOOD_DB[food_type]
    ):
        return FOOD_DB[food_type]["normal"]["maintain"].get(
            day, ("Balanced breakfast", "Balanced lunch", "Balanced dinner")
        )

    return ("Balanced breakfast", "Balanced lunch", "Balanced dinner")


st.subheader("📅 Weekly Diet & Exercise Plan")

days = ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"]
rows = [days[i:i + 3] for i in range(0, 7, 3)]

for row in rows:
    cols = st.columns(3)
    for col, day in zip(cols, row):
        with col:
            b, l, d = get_recommended_food(day)
            meta = meal_meta(day)

            card_text = f"""
📅 **{day}**

🍽 **Food Recommendation**

🥣 **Breakfast ({meta['Breakfast'][0]})**
{b}
Quantity: *{meta['Breakfast'][1]}*

🍲 **Lunch ({meta['Lunch'][0]})**
{l}
Quantity: *{meta['Lunch'][1]}*

🍽 **Dinner ({meta['Dinner'][0]})**
{d}
Quantity: *{meta['Dinner'][1]}*

🏃 **Exercise Recommendation**
{EXERCISE[lifestyle]}
"""
            st.markdown(
                f'<div class="bubble-card">{card_text}</div>',
                unsafe_allow_html=True
            )
