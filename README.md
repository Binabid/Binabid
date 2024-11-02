import discord
from discord.ext import commands

# توكن البوت
TOKEN = 'MTMwMjE5OTIxMTkzMjEyNzI3Mw.G1J8j_.4WisSR9EjKlRzEVP9NSjx4t5Va0KHzuH5Borug'

# تحديد "البادئة" (prefix) للأوامر
bot = commands.Bot(command_prefix='!')

# إعداد القناة التي سيتم فيها إرسال طلبات الدعم الفني
SUPPORT_CHANNEL_ID = 1302292816944431196  # ضع ID قناة الدعم هنا

@bot.event
async def on_ready():
    print(f'Logged in as {bot}')

@bot.command(name='support')
async def support(ctx, *, message: str):
    """يرسل رسالة إلى قناة الدعم الفني"""
    support_channel = bot.get_channel(1302292816944431196)
    if support_channel is None:
        await ctx.send("لا يمكن العثور على قناة الدعم.")
        return

    # إرسال الرسالة لقناة الدعم
    embed = discord.Embed(
        title="طلب دعم فني",
        description=message,
        color=discord.Color.blue()
    )
    embed.set_author(name=ctx.author.name, icon_url=ctx.author.avatar_url)
    await support_channel.send(embed=embed)
    await ctx.send("تم إرسال طلبك للدعم الفني. سيتواصل معك فريقنا قريبًا.")

@bot.event
async def on_message(message):
    # تجاهل رسائل البوت الخاصة
    if message.author == bot.user:
        return

    await bot.process_commands(message)

# تشغيل البوت
bot.run(MTMwMjE5OTIxMTkzMjEyNzI3Mw.G1J8j_.4WisSR9EjKlRzEVP9NSjx4t5Va0KHzuH5Borug)