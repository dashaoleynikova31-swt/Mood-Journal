# Mood Journal 
A simple Python program to track your daily moods.
## Features:
- Add your current mood
- View all recorded moods
- Exit the program
- Provide feedback at the end

print("\nMood Journal\n")

moods = []

while True:  # Display menu options 
    print("1 - Add the mood")
    print("2 - Show all moods")
    print("3 - Exit")

    choice = input("Choose the option: ")

    if choice == "1":
        mood = input("Enter your mood: ")
        moods.append(mood)
        print("Mood saved!\n")

    elif choice == "2":
        if moods:
            print(f"Here are all your moods today: {moods}\n")
        else:
            print("You haven't added any moods yet.\n")

    elif choice == "3":
        print("Bye!")
        break

    else:
        print("Please, enter the correct number!\n")

test = input("Did you like this Mood Journal? ")
if test.lower() in ["yes", "sure", "y", "yeah"]:
    print("You're welcome!")
else:
    print("OK! No problem! I'll practice much better!")







Guys! Here I used AI(Replit)
That looks amasing!

import json
import os
from datetime import datetime, timedelta
import streamlit as st
import matplotlib.pyplot as plt
import pandas as pd

FILENAME = "moods.json"

def load_moods():
    """Завантажує настрої з JSON файлу."""
    if not os.path.exists(FILENAME):
        return []
    with open(FILENAME, "r", encoding="utf-8") as file:
        return json.load(file)

def save_moods(moods):
    """Зберігає настрої у JSON файл."""
    with open(FILENAME, "w", encoding="utf-8") as file:
        json.dump(moods, file, indent=4, ensure_ascii=False)

def add_mood(mood, intensity=5, note=""):
    """Додає новий настрій."""
    entry = {
        "datetime": datetime.now().strftime("%Y-%m-%d %H:%M"),
        "mood": mood,
        "intensity": intensity,
        "note": note
    }
    moods = load_moods()
    moods.append(entry)
    save_moods(moods)

st.set_page_config(
    page_title="Mood Journal 2.0",
    page_icon="🌙",
    layout="centered"
)

st.title("🌙 Mood Journal 2.0")
st.markdown("*Відстежуй свої емоції та аналізуй свій настрій* 💫")

mood_options = {
    "happy": "😊 Happy",
    "sad": "😢 Sad",
    "tired": "😴 Tired",
    "angry": "😠 Angry",
    "calm": "😌 Calm"
}

st.markdown("---")
st.subheader("📝 Як ти себе почуваєш зараз?")

with st.form("mood_form", clear_on_submit=True):
    st.write("**Обери настрій:**")
    
    selected_mood = st.radio(
        "Тип настрою:",
        options=list(mood_options.keys()),
        format_func=lambda x: mood_options[x],
        horizontal=True,
        label_visibility="collapsed"
    )
    
    st.write("**Інтенсивність (1-10):**")
    intensity = st.slider(
        "Наскільки сильно ти це відчуваєш?",
        min_value=1,
        max_value=10,
        value=5,
        help="1 = дуже слабко, 10 = дуже сильно"
    )
    
    note = st.text_area(
        "**Додаткові нотатки (необов'язково):**",
        placeholder="Що сталося? Чому ти так себе почуваєш?",
        max_chars=500
    )
    
    submitted = st.form_submit_button("✨ Зберегти настрій", use_container_width=True, type="primary")
    
    if submitted:
        add_mood(selected_mood, intensity, note)
        st.success(f"✨ Настрій '{selected_mood}' (інтенсивність: {intensity}) збережено!")
        st.rerun()

st.markdown("---")

moods = load_moods()

tab1, tab2, tab3 = st.tabs(["📖 Історія настроїв", "📊 Статистика", "📈 Тренди"])

with tab1:
    st.subheader("📖 Усі твої настрої")
    
    if not moods:
        st.info("😶 Ти ще не додав жодного настрою. Почни зараз!")
    else:
        with st.expander("🔍 Фільтрувати за датою", expanded=False):
            col1, col2 = st.columns(2)
            
            if moods:
                all_dates = [datetime.strptime(m['datetime'], "%Y-%m-%d %H:%M") for m in moods]
                min_date = min(all_dates).date()
                max_date = max(all_dates).date()
            else:
                min_date = datetime.now().date()
                max_date = datetime.now().date()
            
            with col1:
                start_date = st.date_input(
                    "Від:",
                    value=min_date,
                    min_value=min_date,
                    max_value=max_date
                )
            with col2:
                end_date = st.date_input(
                    "До:",
                    value=max_date,
                    min_value=min_date,
                    max_value=max_date
                )
        
        filtered_moods = []
        for entry in moods:
            entry_date = datetime.strptime(entry['datetime'], "%Y-%m-%d %H:%M").date()
            if start_date <= entry_date <= end_date:
                filtered_moods.append(entry)
        
        st.write(f"**Записів у вибраному періоді:** {len(filtered_moods)} з {len(moods)}")
        
        for entry in reversed(filtered_moods):
            mood_emoji = {
                "happy": "😊",
                "sad": "😢", 
                "tired": "😴",
                "angry": "😠",
                "calm": "😌"
            }.get(entry["mood"], "😐")
            
            intensity = entry.get("intensity", "—")
            note = entry.get("note", "")
            
            with st.container():
                col1, col2, col3 = st.columns([2, 1, 1])
                with col1:
                    st.write(f"**{mood_emoji} {entry['mood'].capitalize()}**")
                    st.caption(entry['datetime'])
                with col2:
                    if intensity != "—":
                        st.metric("Інтенсивність", f"{intensity}/10")
                with col3:
                    pass
                
                if note:
                    st.write(f"📝 *{note}*")
                
                st.divider()

with tab2:
    st.subheader("📊 Статистика настроїв")
    
    if not moods:
        st.info("😶 Немає даних для аналізу! Додай кілька настроїв.")
    else:
        mood_counts = {}
        for entry in moods:
            mood = entry["mood"]
            mood_counts[mood] = mood_counts.get(mood, 0) + 1
        
        fig, ax = plt.subplots(figsize=(10, 6))
        colors = ['#FFD93D', '#6BCF7F', '#A8DADC', '#F4A261', '#E76F51']
        bars = ax.bar(mood_counts.keys(), mood_counts.values(), 
                     color=colors[:len(mood_counts)], 
                     edgecolor="black", 
                     linewidth=1.5)
        
        ax.set_title("📊 Частота настроїв", fontsize=16, fontweight='bold', pad=20)
        ax.set_xlabel("Тип настрою", fontsize=12)
        ax.set_ylabel("Кількість", fontsize=12)
        ax.grid(axis="y", linestyle="--", alpha=0.3)
        
        for bar in bars:
            height = bar.get_height()
            ax.text(bar.get_x() + bar.get_width()/2., height,
                   f'{int(height)}',
                   ha='center', va='bottom', fontsize=11, fontweight='bold')
        
        st.pyplot(fig)
        
        col1, col2 = st.columns(2)
        with col1:
            most_common = max(mood_counts, key=mood_counts.get)
            st.metric("Найчастіший настрій", most_common.capitalize(), 
                     f"{mood_counts[most_common]} разів")
        with col2:
            st.metric("Всього записів", len(moods))

with tab3:
    st.subheader("📈 Аналіз трендів")
    
    if not moods:
        st.info("😶 Немає даних для аналізу! Додай кілька настроїв.")
    elif len(moods) < 2:
        st.info("📊 Додай ще кілька настроїв, щоб побачити тренди!")
    else:
        moods_with_intensity = [m for m in moods if m.get("intensity")]
        
        if moods_with_intensity:
            df_trend = pd.DataFrame(moods_with_intensity)
            df_trend['datetime'] = pd.to_datetime(df_trend['datetime'], format="%Y-%m-%d %H:%M")
            df_trend = df_trend.sort_values('datetime')
            
            fig, (ax1, ax2) = plt.subplots(2, 1, figsize=(12, 10))
            
            mood_colors = {
                'happy': '#FFD93D',
                'sad': '#6BCF7F', 
                'tired': '#A8DADC',
                'angry': '#F4A261',
                'calm': '#E76F51'
            }
            
            for mood_type in df_trend['mood'].unique():
                mood_data = df_trend[df_trend['mood'] == mood_type]
                ax1.plot(mood_data['datetime'], mood_data['intensity'], 
                        marker='o', label=mood_type.capitalize(), 
                        color=mood_colors.get(mood_type, '#888888'),
                        linewidth=2, markersize=6)
            
            ax1.set_title("📈 Інтенсивність настроїв з часом", fontsize=14, fontweight='bold', pad=15)
            ax1.set_xlabel("Дата", fontsize=11)
            ax1.set_ylabel("Інтенсивність (1-10)", fontsize=11)
            ax1.legend(loc='best')
            ax1.grid(True, alpha=0.3)
            ax1.set_ylim(0, 11)
            
            daily_avg = df_trend.groupby(df_trend['datetime'].dt.date)['intensity'].mean()
            ax2.plot(daily_avg.index, daily_avg.values, 
                    marker='o', color='#6BCF7F', linewidth=2.5, markersize=8)
            ax2.fill_between(daily_avg.index, daily_avg.values, alpha=0.3, color='#6BCF7F')
            
            ax2.set_title("📊 Середня денна інтенсивність", fontsize=14, fontweight='bold', pad=15)
            ax2.set_xlabel("Дата", fontsize=11)
            ax2.set_ylabel("Середня інтенсивність", fontsize=11)
            ax2.grid(True, alpha=0.3)
            ax2.set_ylim(0, 11)
            
            plt.tight_layout()
            st.pyplot(fig)
            
            col1, col2, col3 = st.columns(3)
            with col1:
                avg_intensity = df_trend['intensity'].mean()
                st.metric("Середня інтенсивність", f"{avg_intensity:.1f}/10")
            with col2:
                max_intensity = df_trend['intensity'].max()
                st.metric("Максимальна інтенсивність", f"{max_intensity}/10")
            with col3:
                min_intensity = df_trend['intensity'].min()
                st.metric("Мінімальна інтенсивність", f"{min_intensity}/10")
        else:
            st.info("📊 Для аналізу трендів потрібні дані про інтенсивність. Додай нові настрої з інтенсивністю!")

st.markdown("---")

st.subheader("🧹 Керування даними")

col_export, col_clear = st.columns(2)

with col_export:
    if moods:
        df_export = pd.DataFrame(moods)
        csv = df_export.to_csv(index=False, encoding='utf-8-sig')
        st.download_button(
            label="📥 Експортувати в CSV",
            data=csv,
            file_name=f"mood_journal_{datetime.now().strftime('%Y%m%d')}.csv",
            mime="text/csv",
            use_container_width=True
        )
    else:
        st.button("📥 Експортувати в CSV", disabled=True, use_container_width=True)

with col_clear:
    if os.path.exists(FILENAME):
        if st.button("🗑️ Очистити всі дані", type="secondary", use_container_width=True):
            st.session_state.confirm_delete = True
    else:
        st.button("🗑️ Очистити всі дані", disabled=True, use_container_width=True)

if 'confirm_delete' in st.session_state and st.session_state.confirm_delete:
    st.warning("⚠️ Ти впевнений? Це видалить усі твої настрої назавжди!")
    col1, col2, col3 = st.columns([1, 1, 2])
    with col1:
        if st.button("✅ Так, видалити"):
            os.remove(FILENAME)
            st.session_state.confirm_delete = False
            st.success("🧹 Усі настрої видалено!")
            st.rerun()
    with col2:
        if st.button("❌ Скасувати"):
            st.session_state.confirm_delete = False
            st.rerun()

st.markdown("---")
st.markdown(
    "<div style='text-align: center; color: #888;'>"
    "💖 Пам'ятай — твій настрій важливий 💖"
    "</div>", 
    unsafe_allow_html=True
)
