import fetch from "node-fetch";

export default async function handler(req, res) {
  try {
    const { q } = req.query;

    if (!q) {
      return res.status(400).json({ error: "Missing search query" });
    }

    const url = `https://graph.facebook.com/v19.0/ads_archive?search_terms=${encodeURIComponent(
      q
    )}&ad_type=HOUSING_ADS&fields=ad_creative_bodies,ad_creative_link_titles,ad_snapshot_url,page_name&access_token=${process.env.FB_TOKEN}`;

    const r = await fetch(url);
    const data = await r.json();

    res.setHeader("Access-Control-Allow-Origin", "*"); // allow frontend
    res.status(200).json(data);
  } catch (e) {
    console.error(e);
    res.status(500).json({ error: "Something went wrong" });
  }
}
