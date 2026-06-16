import yfinance as yf
from openai import OpenAI
import os

def get_client():
    import streamlit as st
    api_key = st.secrets.get("OPENAI_API_KEY")
    return OpenAI(api_key=api_key)

def get_stock_news(ticker):
    stock = yf.Ticker(ticker)
    return stock.news

def analyze_news_for_longterm(news_list):
    client = get_client()
    news_summary = ""
    for n in news_list[:5]:
        news_summary += f"- 제목: {n['title']}\n  요약: {n.get('summary', '')}\n\n"
    
    prompt = f"""
    당신은 세계 최고의 가치투자 전문가입니다. 
    다음 뉴스들을 읽고, 이 기업이 '3년 이상 장기 투자' 관점에서 호재인지 악재인지 분석해주세요.
    단기 테마성 이슈는 무시하고, 기업의 구조적 성장이나 경쟁력(해자)에 미치는 영향 위주로 판단하세요.
    
    [뉴스 데이터]
    {news_summary}
    
    [반드시 아래 형식으로만 답변하세요]
    ■ 장기 투자 추천도: (매우 높음 / 높음 / 보통 / 낮음 중 선택)
    ■ 핵심 요약: 
    ■ 상세 분석 이유: 
    """
    
    response = client.chat.completions.create(
        model="gpt-4o-mini",
        messages=[{"role": "user", "content": prompt}]
    )
    return response.choices[0].message.content
