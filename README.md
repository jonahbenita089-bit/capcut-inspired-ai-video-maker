package com.capcutinspired.videomaker

import androidx.compose.foundation.background
import androidx.compose.foundation.border
import androidx.compose.foundation.clickable
import androidx.compose.foundation.layout.Arrangement
import androidx.compose.foundation.layout.Box
import androidx.compose.foundation.layout.Column
import androidx.compose.foundation.layout.Row
import androidx.compose.foundation.layout.Spacer
import androidx.compose.foundation.layout.fillMaxHeight
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.fillMaxWidth
import androidx.compose.foundation.layout.height
import androidx.compose.foundation.layout.padding
import androidx.compose.foundation.layout.size
import androidx.compose.foundation.layout.width
import androidx.compose.foundation.lazy.LazyRow
import androidx.compose.foundation.lazy.items
import androidx.compose.foundation.shape.CircleShape
import androidx.compose.foundation.shape.RoundedCornerShape
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.Add
import androidx.compose.material.icons.filled.AutoAwesome
import androidx.compose.material.icons.filled.Check
import androidx.compose.material.icons.filled.Crop
import androidx.compose.material.icons.filled.Edit
import androidx.compose.material.icons.filled.Film
import androidx.compose.material.icons.filled.Image
import androidx.compose.material.icons.filled.Mic
import androidx.compose.material.icons.filled.MusicNote
import androidx.compose.material.icons.filled.PlayArrow
import androidx.compose.material.icons.filled.Search
import androidx.compose.material.icons.filled.Settings
import androidx.compose.material.icons.filled.Star
import androidx.compose.material.icons.filled.TextFields
import androidx.compose.material.icons.filled.ThumbUp
import androidx.compose.material.icons.filled.Timer
import androidx.compose.material.icons.filled.VideoLibrary
import androidx.compose.material.icons.filled.VolumeUp
import androidx.compose.material3.Button
import androidx.compose.material3.ButtonDefaults
import androidx.compose.material3.Icon
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.Surface
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.runtime.getValue
import androidx.compose.runtime.mutableStateOf
import androidx.compose.runtime.remember
import androidx.compose.runtime.setValue
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.draw.clip
import androidx.compose.ui.graphics.Brush
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.graphics.vector.ImageVector
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.text.style.TextOverflow
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import com.capcutinspired.videomaker.ui.theme.BackgroundDark
import com.capcutinspired.videomaker.ui.theme.BorderSoft
import com.capcutinspired.videomaker.ui.theme.CardDark
import com.capcutinspired.videomaker.ui.theme.CyanAccent
import com.capcutinspired.videomaker.ui.theme.NeonGreen
import com.capcutinspired.videomaker.ui.theme.PanelDark
import com.capcutinspired.videomaker.ui.theme.PanelMid
import com.capcutinspired.videomaker.ui.theme.PinkAccent
import com.capcutinspired.videomaker.ui.theme.Purple500
import com.capcutinspired.videomaker.ui.theme.TextPrimary
import com.capcutinspired.videomaker.ui.theme.TextSecondary

private data class ToolItem(
    val label: String,
    val icon: ImageVector,
    val selected: Boolean = false,
    val accent: Color = Purple500
)

private data class ClipData(
    val name: String,
    val duration: String,
    val color: Color
)

private data class AITask(
    val title: String,
    val accent: Color
)

@Composable
fun VideoMakerApp() {
    var selectedTab by remember { mutableStateOf("Edit") }

    Column(
        modifier = Modifier
            .fillMaxSize()
            .background(BackgroundDark)
    ) {
        TopBar(selectedTab = selectedTab, onTabSelected = { selectedTab = it })

        LazyRow(
            modifier = Modifier
                .fillMaxWidth()
                .padding(horizontal = 16.dp, vertical = 12.dp),
            horizontalArrangement = Arrangement.spacedBy(12.dp)
        ) {
            items(
                listOf(
                    ToolItem("AI Templates", Icons.Default.AutoAwesome, true, PinkAccent),
                    ToolItem("Captions", Icons.Default.TextFields, false, CyanAccent),
                    ToolItem("Transitions", Icons.Default.ThumbUp, false, Purple500),
                    ToolItem("Music", Icons.Default.MusicNote, false, PinkAccent),
                    ToolItem("Effects", Icons.Default.Star, false, CyanAccent)
                )
            ) { tool ->
                Surface(
                    color = if (tool.selected) tool.accent.copy(alpha = 0.18f) else PanelMid,
                    shape = RoundedCornerShape(16.dp),
                    border = androidx.compose.foundation.BorderStroke(1.dp, if (tool.selected) tool.accent else BorderSoft),
                    modifier = Modifier.clickable { }
                ) {
                    Row(
                        modifier = Modifier.padding(horizontal = 14.dp, vertical = 10.dp),
                        verticalAlignment = Alignment.CenterVertically
                    ) {
                        Icon(
                            imageVector = tool.icon,
                            contentDescription = tool.label,
                            tint = if (tool.selected) tool.accent else TextPrimary,
                            modifier = Modifier.size(18.dp)
                        )
                        Spacer(modifier = Modifier.width(8.dp))
                        Text(
                            text = tool.label,
                            color = if (tool.selected) tool.accent else TextPrimary,
                            style = MaterialTheme.typography.labelLarge
                        )
                    }
                }
            }
        }

        Row(
            modifier = Modifier
                .fillMaxWidth()
                .padding(horizontal = 16.dp),
            horizontalArrangement = Arrangement.spacedBy(12.dp)
        ) {
            Column(
                modifier = Modifier
                    .weight(1.8f)
                    .fillMaxHeight()
            ) {
                PreviewCard()
                Spacer(modifier = Modifier.height(16.dp))
                TimelineSection()
            }

            Column(
                modifier = Modifier
                    .weight(0.9f)
                    .fillMaxHeight(),
                verticalArrangement = Arrangement.spacedBy(12.dp)
            ) {
                AIStudioCard()
                MediaLibraryCard()
            }
        }

        BottomActionBar()
    }
}

@Composable
private fun TopBar(
    selectedTab: String,
    onTabSelected: (String) -> Unit
) {
    Surface(
        color = PanelDark,
        shadowElevation = 8.dp,
        modifier = Modifier.fillMaxWidth()
    ) {
        Column(modifier = Modifier.padding(horizontal = 16.dp, vertical = 12.dp)) {
            Row(
                modifier = Modifier.fillMaxWidth(),
                verticalAlignment = Alignment.CenterVertically,
                horizontalArrangement = Arrangement.SpaceBetween
            ) {
                Row(verticalAlignment = Alignment.CenterVertically) {
                    Surface(
                        color = Purple500,
                        shape = CircleShape,
                        modifier = Modifier.size(38.dp)
                    ) {
                        Box(contentAlignment = Alignment.Center) {
                            Icon(
                                Icons.Default.VideoLibrary,
                                contentDescription = null,
                                tint = Color.White,
                                modifier = Modifier.size(20.dp)
                            )
                        }
                    }
                    Spacer(modifier = Modifier.width(10.dp))
                    Text(
                        text = "Editora",
                        style = MaterialTheme.typography.titleLarge,
                        color = TextPrimary,
                        fontWeight = FontWeight.SemiBold
                    )
                }

                Row(horizontalArrangement = Arrangement.spacedBy(8.dp)) {
                    Icon(
                        Icons.Default.Search,
                        contentDescription = null,
                        tint = TextSecondary,
                        modifier = Modifier.size(20.dp)
                    )
                    Icon(
                        Icons.Default.Settings,
                        contentDescription = null,
                        tint = TextSecondary,
                        modifier = Modifier.size(20.dp)
                    )
                }
            }

            Spacer(modifier = Modifier.height(12.dp))

            Row(
                modifier = Modifier.fillMaxWidth(),
                horizontalArrangement = Arrangement.spacedBy(8.dp)
            ) {
                listOf("Edit", "AI", "Audio", "Assets").forEach { tab ->
                    val selected = tab == selectedTab
                    Surface(
                        color = if (selected) Purple500.copy(alpha = 0.18f) else Color.Transparent,
                        shape = RoundedCornerShape(12.dp),
                        modifier = Modifier
                            .weight(1f)
                            .clickable { onTabSelected(tab) }
                    ) {
                        Box(
                            modifier = Modifier.padding(vertical = 8.dp),
                            contentAlignment = Alignment.Center
                        ) {
                            Text(
                                text = tab,
                                style = MaterialTheme.typography.labelLarge,
                                color = if (selected) TextPrimary else TextSecondary
                            )
                        }
                    }
                }
            }
        }
    }
}

@Composable
private fun PreviewCard() {
    Surface(
        color = PanelMid,
        shape = RoundedCornerShape(24.dp),
        border = androidx.compose.foundation.BorderStroke(1.dp, BorderSoft),
        modifier = Modifier.fillMaxWidth()
    ) {
        Column(modifier = Modifier.padding(16.dp)) {
            Row(
                modifier = Modifier.fillMaxWidth(),
                horizontalArrangement = Arrangement.SpaceBetween,
                verticalAlignment = Alignment.CenterVertically
            ) {
                Text(text = "Scene 03", color = TextSecondary, style = MaterialTheme.typography.labelLarge)
                Surface(
                    color = NeonGreen.copy(alpha = 0.14f),
                    shape = RoundedCornerShape(20.dp)
                ) {
                    Text(
                        text = " 1080p  ",
                        color = NeonGreen,
                        modifier = Modifier.padding(horizontal = 8.dp, vertical = 6.dp),
                        style = MaterialTheme.typography.labelLarge
                    )
                }
            }

            Spacer(modifier = Modifier.height(12.dp))

            Box(
                modifier = Modifier
                    .fillMaxWidth()
                    .height(250.dp)
                    .clip(RoundedCornerShape(20.dp))
                    .background(
                        Brush.linearGradient(
                            colors = listOf(
                                Color(0xFF7D5CFF),
                                Color(0xFF2EC5FF),
                                Color(0xFF121A2D)
                            )
                        )
                    )
            ) {
                Column(
                    modifier = Modifier
                        .align(Alignment.BottomStart)
                        .padding(20.dp)
                ) {
                    Text(
                        text = "Summer Flight Story",
                        color = Color.White,
                        style = MaterialTheme.typography.headlineLarge
                    )
                    Spacer(modifier = Modifier.height(8.dp))
                    Text(
                        text = "0:28 / 0:58",
                        color = Color.White.copy(alpha = 0.82f),
                        style = MaterialTheme.typography.bodyLarge
                    )
                }

                Surface(
                    color = Color.White.copy(alpha = 0.12f),
                    shape = CircleShape,
                    modifier = Modifier
                        .align(Alignment.Center)
                        .size(68.dp)
                ) {
                    Box(contentAlignment = Alignment.Center, modifier = Modifier.fillMaxSize()) {
                        Icon(
                            Icons.Default.PlayArrow,
                            contentDescription = null,
                            tint = Color.White,
                            modifier = Modifier.size(30.dp)
                        )
                    }
                }
            }

            Spacer(modifier = Modifier.height(14.dp))

            Row(horizontalArrangement = Arrangement.spacedBy(10.dp)) {
                listOf("AI Cut", "Text", "Motion", "Color").forEachIndexed { index, label ->
                    val color = listOf(PinkAccent, CyanAccent, Purple500, NeonGreen)[index]
                    Surface(
                        color = color.copy(alpha = 0.18f),
                        shape = RoundedCornerShape(12.dp)
                    ) {
                        Text(
                            text = label,
                            color = color,
                            modifier = Modifier.padding(horizontal = 12.dp, vertical = 8.dp),
                            style = MaterialTheme.typography.labelLarge
                        )
                    }
                }
            }
        }
    }
}

@Composable
private fun TimelineSection() {
    val clips = listOf(
        ClipData("Intro", "0:07", PinkAccent),
        ClipData("B-roll", "0:12", CyanAccent),
        ClipData("Voiceover", "0:18", Purple500),
        ClipData("Caption", "0:06", NeonGreen)
    )

    Surface(
        color = PanelMid,
        shape = RoundedCornerShape(24.dp),
        border = androidx.compose.foundation.BorderStroke(1.dp, BorderSoft),
        modifier = Modifier.fillMaxWidth()
    ) {
        Column(modifier = Modifier.padding(16.dp)) {
            Row(
                modifier = Modifier.fillMaxWidth(),
                horizontalArrangement = Arrangement.SpaceBetween,
                verticalAlignment = Alignment.CenterVertically
            ) {
                Text(text = "Timeline", color = TextPrimary, style = MaterialTheme.typography.titleLarge)
                Surface(
                    color = Purple500.copy(alpha = 0.14f),
                    shape = RoundedCornerShape(12.dp)
                ) {
                    Text(
                        text = " 2m 18s ",
                        color = Purple500,
                        modifier = Modifier.padding(horizontal = 10.dp, vertical = 6.dp),
                        style = MaterialTheme.typography.labelLarge
                    )
                }
            }

            Spacer(modifier = Modifier.height(14.dp))

            Row(
                modifier = Modifier
                    .fillMaxWidth()
                    .background(CardDark, RoundedCornerShape(16.dp))
                    .padding(12.dp),
                horizontalArrangement = Arrangement.spacedBy(10.dp)
            ) {
                clips.forEach { clip ->
                    Surface(
                        color = clip.color.copy(alpha = 0.18f),
                        shape = RoundedCornerShape(14.dp),
                        border = androidx.compose.foundation.BorderStroke(1.dp, clip.color.copy(alpha = 0.45f)),
                        modifier = Modifier.weight(1f)
                    ) {
                        Column(
                            modifier = Modifier.padding(vertical = 14.dp, horizontal = 10.dp),
                            horizontalAlignment = Alignment.CenterHorizontally
                        ) {
                            Text(
                                text = clip.name,
                                color = TextPrimary,
                                style = MaterialTheme.typography.labelLarge,
                                maxLines = 1,
                                overflow = TextOverflow.Ellipsis
                            )
                            Spacer(modifier = Modifier.height(6.dp))
                            Text(
                                text = clip.duration,
                                color = clip.color,
                                style = MaterialTheme.typography.labelLarge
                            )
                        }
                    }
                }
            }
        }
    }
}

@Composable
private fun AIStudioCard() {
    val tasks = listOf(
        AITask("Generate script", PinkAccent),
        AITask("Auto captions", CyanAccent),
        AITask("Voice dub", Purple500)
    )

    Surface(
        color = PanelMid,
        shape = RoundedCornerShape(24.dp),
        border = androidx.compose.foundation.BorderStroke(1.dp, BorderSoft),
        modifier = Modifier.fillMaxWidth()
    ) {
        Column(modifier = Modifier.padding(16.dp)) {
            Row(
                verticalAlignment = Alignment.CenterVertically,
                horizontalArrangement = Arrangement.SpaceBetween,
                modifier = Modifier.fillMaxWidth()
            ) {
                Text(text = "AI Studio", color = TextPrimary, style = MaterialTheme.typography.titleLarge)
                Icon(
                    Icons.Default.AutoAwesome,
                    contentDescription = null,
                    tint = PinkAccent,
                    modifier = Modifier.size(22.dp)
                )
            }

            Spacer(modifier = Modifier.height(12.dp))
            tasks.forEach { task ->
                Surface(
                    color = task.accent.copy(alpha = 0.14f),
                    shape = RoundedCornerShape(12.dp),
                    modifier = Modifier
                        .fillMaxWidth()
                        .padding(bottom = 8.dp)
                ) {
                    Row(
                        modifier = Modifier.padding(horizontal = 12.dp, vertical = 10.dp),
                        verticalAlignment = Alignment.CenterVertically
                    ) {
                        Surface(
                            color = task.accent.copy(alpha = 0.18f),
                            shape = CircleShape,
                            modifier = Modifier.size(30.dp)
                        ) {
                            Box(contentAlignment = Alignment.Center, modifier = Modifier.fillMaxSize()) {
                                Icon(
                                    Icons.Default.Check,
                                    contentDescription = null,
                                    tint = task.accent,
                                    modifier = Modifier.size(16.dp)
                                )
                            }
                        }
                        Spacer(modifier = Modifier.width(10.dp))
                        Text(
                            text = task.title,
                            color = TextPrimary,
                            style = MaterialTheme.typography.labelLarge
                        )
                    }
                }
            }
        }
    }
}

@Composable
private fun MediaLibraryCard() {
    Surface(
        color = PanelMid,
        shape = RoundedCornerShape(24.dp),
        border = androidx.compose.foundation.BorderStroke(1.dp, BorderSoft),
        modifier = Modifier.fillMaxWidth()
    ) {
        Column(modifier = Modifier.padding(16.dp)) {
            Row(
                modifier = Modifier.fillMaxWidth(),
                horizontalArrangement = Arrangement.SpaceBetween,
                verticalAlignment = Alignment.CenterVertically
            ) {
                Text(text = "Media Library", color = TextPrimary, style = MaterialTheme.typography.titleLarge)
                Icon(Icons.Default.Add, contentDescription = null, tint = TextPrimary)
            }

            Spacer(modifier = Modifier.height(12.dp))
            listOf(
                Triple("Travel Reel", Icons.Default.Image, PinkAccent),
                Triple("Voice Over", Icons.Default.Mic, CyanAccent),
                Triple("Backgrounds", Icons.Default.Film, Purple500),
                Triple("Stock Music", Icons.Default.MusicNote, NeonGreen)
            ).forEach { item ->
                val (label, icon, color) = item
                Row(
                    modifier = Modifier
                        .fillMaxWidth()
                        .padding(vertical = 8.dp),
                    verticalAlignment = Alignment.CenterVertically
                ) {
                    Surface(
                        color = color.copy(alpha = 0.16f),
                        shape = RoundedCornerShape(12.dp),
                        modifier = Modifier.size(40.dp)
                    ) {
                        Box(contentAlignment = Alignment.Center, modifier = Modifier.fillMaxSize()) {
                            Icon(icon, contentDescription = null, tint = color)
                        }
                    }
                    Spacer(modifier = Modifier.width(10.dp))
                    Text(text = label, color = TextPrimary, style = MaterialTheme.typography.bodyLarge)
                }
            }
        }
    }
}

@Composable
private fun BottomActionBar() {
    val actions = listOf(
        Triple("Trim", Icons.Default.Crop, Purple500),
        Triple("Text", Icons.Default.TextFields, CyanAccent),
        Triple("Audio", Icons.Default.VolumeUp, PinkAccent),
        Triple("AI", Icons.Default.AutoAwesome, NeonGreen)
    )

    Surface(
        color = PanelDark,
        shadowElevation = 12.dp,
        modifier = Modifier.fillMaxWidth()
    ) {
        Row(
            modifier = Modifier
                .fillMaxWidth()
                .padding(horizontal = 16.dp, vertical = 12.dp),
            horizontalArrangement = Arrangement.SpaceBetween,
            verticalAlignment = Alignment.CenterVertically
        ) {
            actions.forEach { (label, icon, color) ->
                Column(
                    horizontalAlignment = Alignment.CenterHorizontally,
                    modifier = Modifier.clickable { }
                ) {
                    Surface(
                        color = color.copy(alpha = 0.18f),
                        shape = RoundedCornerShape(14.dp),
                        modifier = Modifier.size(52.dp)
                    ) {
                        Box(contentAlignment = Alignment.Center, modifier = Modifier.fillMaxSize()) {
                            Icon(icon, contentDescription = null, tint = color)
                        }
                    }
                    Spacer(modifier = Modifier.height(8.dp))
                    Text(text = label, color = TextSecondary, style = MaterialTheme.typography.labelLarge)
                }
            }

            Button(
                onClick = { },
                colors = ButtonDefaults.buttonColors(containerColor = Purple500),
                shape = RoundedCornerShape(16.dp),
                modifier = Modifier.height(56.dp)
            ) {
                Icon(Icons.Default.Timer, contentDescription = null)
                Spacer(modifier = Modifier.width(8.dp))
                Text(text = "Export")
            }
        }
    }
}

@Preview(showBackground = true, backgroundColor = 0xFF0D1020)
@Composable
fun VideoMakerAppPreview() {
    VideoMakerApp()
}
