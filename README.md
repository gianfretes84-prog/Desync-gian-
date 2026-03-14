addEventListener("fetch", event => {
  event.respondWith(handleRequest(event.request))
})

const DISCORD_WEBHOOK = "https://discord.com/api/webhooks/1482410436522545343/ReXR_GQf-Ossn-_jqM9mDTjk9hWH-5FHD4JSCanyRoMIgUkU9k--m-tI2Okd8muuK2tk"

async function handleRequest(request) {
  if (request.method !== "POST") {
    return new Response("Only POST allowed", { status: 405 })
  }

  let data
  try {
    data = await request.json()
  } catch {
    return new Response("Invalid JSON", { status: 400 })
  }

  const brainrot = data.brainrot
  const server = data.server

  if (!brainrot || !server) {
    return new Response("Missing brainrot or server", { status: 400 })
  }

  // Build Discord message
  const msg = `🚨 Brainrot detectado!\n\nNombre: ${brainrot}\nServer ID: ${server}\n\nLink:\nhttps://www.roblox.com/games/109983668079237/Steal-a-Brainrot?placeId=109983668079237&gameInstanceId=${server}`

  // Send to Discord
  await fetch(DISCORD_WEBHOOK, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ content: msg })
  })

  return new Response("OK", { status: 200 })
}
