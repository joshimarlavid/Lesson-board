import streamlit as st
import time

# Configuración de la página
st.set_page_config(page_title="ESL Mystery Lesson", page_icon="🕵️‍♀️", layout="centered")

# Título Principal
st.title("🕵️‍♀️ Mystery, Travel & Unexpected Events")
st.write("Welcome! Today we practice **Past Simple** vs. **Past Continuous** without talking about family trees!")

# Crear pestañas para las secciones
tab1, tab2, tab3, tab4 = st.tabs(["1. Warm-up", "2. The Alibi Game", "3. Travel Story", "4. Grammar Focus"])

# --- SECCIÓN 1: WARM-UP ---
with tab1:
    st.header("🤝 Famous Duos")
    st.write("Look at these famous pairs. Use your imagination: **How did they meet?**")
    
    col1, col2 = st.columns(2)
    with col1:
        st.info("**Sherlock Holmes & Dr. Watson**")
        user_theory_1 = st.text_input("Your theory for Holmes & Watson:", placeholder="E.g. They met while...")
    
    with col2:
        st.info("**Han Solo & Chewbacca**")
        user_theory_2 = st.text_input("Your theory for Han Solo & Chewie:", placeholder="E.g. Han was flying when...")

    if st.button("Show Possible Answers", key="warmup_btn"):
        st.success("✨ **Possible Answer 1:** They met **while** Watson **was looking** for an apartment in London.")
        st.success("✨ **Possible Answer 2:** They met **when** Han **was trying** to escape from the Empire.")

# --- SECCIÓN 2: THE ALIBI GAME ---
with tab2:
    st.header("🕵️‍♂️ The Alibi Game")
    st.warning("⚠️ A CRIME happened yesterday between **6:00 PM and 8:00 PM**. You are a suspect!")
    st.write("The Detective will ask you what you were doing. **Be specific!**")

    # Inicializar historial de chat
    if "messages" not in st.session_state:
        st.session_state.messages = [{"role": "assistant", "content": "Where were you yesterday at 7:00 PM exactly? I know you weren't at home!"}]

    # Mostrar mensajes anteriores
    for msg in st.session_state.messages:
        role = "🕵️ Detective" if msg["role"] == "assistant" else "👤 You"
        st.chat_message(msg["role"]).write(msg["content"])

    # Input del usuario
    if prompt := st.chat_input("Type your alibi here (e.g., I was cooking dinner...)"):
        st.session_state.messages.append({"role": "user", "content": prompt})
        st.chat_message("user").write(prompt)

        # Lógica simple del "Detective" (Respuestas automáticas)
        response = ""
        prompt_lower = prompt.lower()
        
        if "was" in prompt_lower or "were" in prompt_lower:
             response = "Hmph. Likely story. And **who were you talking to** while you were doing that?"
        elif "didn't" in prompt_lower or "don't" in prompt_lower:
             response = "Stop denying it! Tell me what you **were doing** when the phone rang!"
        else:
             response = "Use the correct grammar! Tell me: 'I **was** doing something...' Try again!"
        
        # Añadir un poco de aleatoriedad para que no sea repetitivo
        if len(st.session_state.messages) > 4:
            response = "Alright, alright. I believe you... for now. Stay in town."

        # Simular tiempo de pensar
        with st.spinner("Detective is suspicious..."):
            time.sleep(1)
            
        st.session_state.messages.append({"role": "assistant", "content": response})
        st.chat_message("assistant").write(response)

# --- SECCIÓN 3: STORY TIME ---
with tab3:
    st.header("✈️ The Travel Mishap")
    st.image("https://images.unsplash.com/photo-1436491865332-7a61a109cc05", caption="Airport Lounge", width=400)
    
    st.markdown("""
    **Read the story:**
    
    > "I met my current business partner, Sarah, five years ago. It was a complete disaster at first. 
    > I **was waiting** for my flight to Tokyo at O'Hare Airport when suddenly the announcement **cancelled** all flights due to a snowstorm.
    >
    > Everyone was angry. While I **was arguing** with the airline agent, Sarah **tapped** me on the shoulder. 
    > She offered me a spare charger because my phone **was dying**. We ended up having dinner while we **were waiting** for the snow to stop."
    """)
    
    st.divider()
    st.subheader("Comprehension Check")
    
    q1 = st.radio("1. What was the narrator doing when the flight was cancelled?", 
                  ["He was flying to Tokyo.", "He was waiting at the airport.", "He was having dinner."])
    
    q2 = st.radio("2. When did Sarah tap him on the shoulder?", 
                  ["While he was sleeping.", "When he arrived.", "While he was arguing with the agent."])
    
    if st.button("Check Answers", key="story_btn"):
        score = 0
        if "waiting" in q1: score += 1
        if "arguing" in q2: score += 1
        
        if score == 2:
            st.balloons()
            st.success("Correct! Great job identifying the background actions.")
        else:
            st.error("Not quite. Remember: Past Continuous describes the action in progress.")

# --- SECCIÓN 4: GRAMMAR FOCUS ---
with tab4:
    st.header("🧠 Grammar: During vs. While")
    st.info("Remember: **During + Noun** | **While + Subject + Verb**")
    
    with st.form("grammar_form"):
        st.write("Complete the sentences:")
        
        a1 = st.selectbox("1. Please don't look at your phone ___ the meeting.", ["during", "while"])
        a2 = st.selectbox("2. I fell asleep ___ I was watching the presentation.", ["during", "while"])
        a3 = st.selectbox("3. ___ the fire alarm rang, we were coding.", ["During", "When"])
        
        submitted = st.form_submit_button("Submit")
        
        if submitted:
            if a1 == "during" and a2 == "while" and a3 == "When":
                st.success("Perfect score! 💯")
            else:
                st.warning("Check your answers again. Remember the rule!")

