import streamlit as st
from openai import OpenAI

client = OpenAI(api_key=st.secrets["OPENAI_API_KEY"])

st.title("🎵 기분 맞춤 플레이리스트 생성기")

# 기분 입력
mood = st.text_input("지금 기분을 말해줘! (예: 기분이 안 좋아, 행복해, 우울해, 설레 등)")

# 버튼
if st.button("플레이리스트 만들기"):
    
    # GPT에게 메시지 요청
    chat_completion = client.chat.completions.create(
        model="gpt-4o-mini",
        messages=[
            {
                "role": "system",
                "content":
                """
                사용자의 기분을 분석해서 다음을 생성해줘:

                1) 감정에 딱 맞는 짧은 위로/응원/격려 문구 1개
                2) 기분에 맞는 노래 추천 5곡
                   - 형식: 제목 / 가수 / 추천 이유(짧게)

                말투는 따뜻하고 친절하게.
                """
            },
            {
                "role": "user",
                "content": f"내 기분: {mood}"
            }
        ]
    )

    result = chat_completion.choices[0].message.content

    st.subheader("💬 오늘의 감정 메시지 + 플레이리스트")
    st.write(result)
