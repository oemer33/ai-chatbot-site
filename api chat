export default async function handler(req, res) {
  if (req.method !== 'POST') {
    res.setHeader('Allow', ['POST']);
    return res.status(405).json({ error: 'Method not allowed' });
  }

  try {
    const { messages } = req.body || {};
    if (!Array.isArray(messages)) {
      return res.status(400).json({ error: 'Invalid payload' });
    }

    const apiKey = process.env.OPENAI_API_KEY;
    if (!apiKey) {
      return res.status(500).json({ error: 'API key not configured' });
    }

    const openaiRes = await fetch('https://api.openai.com/v1/chat/completions', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        Authorization: `Bearer ${apiKey}`
      },
      body: JSON.stringify({
        model: 'gpt-4.1-mini',
        messages,
        temperature: 0.4
      })
    });

    if (!openaiRes.ok) {
      const errorText = await openaiRes.text();
      console.error('OpenAI API error:', errorText);
      return res.status(500).json({ error: 'AI request failed' });
    }

    const result = await openaiRes.json();
    const reply =
      result.choices?.[0]?.message?.content?.trim() ||
      'Keine Antwort erhalten.';

    return res.status(200).json({ reply });
  } catch (error) {
    console.error('Server error:', error);
    return res.status(500).json({ error: 'Server error' });
  }
}
